# Using EC2 to Build a Video Chat Recording Service.

You’ll continue to use vidcall SDK to add group video chat to your web/mobile/desktop application — no changes required there. However, if you want to take advantage of recording, broadcast, or interoperability , you can incorporate the media bridge into your application. As the name suggests, the media bridge allows you to build a bridge from your vidcall video conference to other destinations.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/How-To-Record-001.jpg)

## Step 1. Set Up AWS EC2 to Run the Media Bridge

Docker is in no way, shape, or form limited to AWS, and you can deploy it on any Linux, Windows, or other popular cloud server service. I picked AWS because it’s one of the most popular cloud services, it offers a free tier so you can try the setup at no cost, and it provides first-class Docker support via the EC2 Container Service (ECS), which can be easily scaled to meet your demands.

* To get started, log in to your AWS Console, click the “EC2” link to go to the EC2 Console, and click the blue “Launch Instance” button.

* Select the Amazon Linux machine, which is eligible for the free tier. You’ll see in the description that it already includes some useful   command line tools and the Docker repository.

* Next, for the instance type, choose t2.micro.

* For our media bridge to connect to a video conference call and receive media packets we’ll enable all the ports for TCP and UDP. Let’s     create a new security group, give it a name, and add all TCP and UDP traffic.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(182).png)

* Click the blue “Review and Launch” button.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(183).png)

* If everything looks good, you’re now ready to connect to your cloud server to deploy the media bridge instance.

## Step 2. Deploying the Media Bridge

To deploy the media bridge on our EC2 instance, we’ll first connect to our instance using SSH.

### For Linux user:

Open a terminal and go to the folder where you saved your key pair. Set read-only access to the key pair file using chmod:

```bash
chmod 400 vediorecorder.pem
ssh -i ec2-key-pair.pem ec2-user@<EC2-INSTANCE-PUBLIC-IP-ADDRESS>
```

### For Windows user:

Windows use can directly connect using PuTTY.

To install Docker, run the following command:

```bash
sudo yum update -y
sudo yum install -y docker
sudo service docker start
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(150).png)

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(167).png)

Once Docker is installed, you can pull the latest media bridge version and run a container with the following commands:

```bash
sudo docker pull vidyo/mediabridge
sudo docker run -d — name my_mediabridge vidyo/mediabridge
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(170).png)

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(171).png)

## Step 3. Create a Simple Web Service

Now you have the media bridge instance deployed and you’re almost ready to connect to a call.

Before it can connect it needs three things:
* A valid token
* The name of the resourceId to connect to
* A destination, whether it is going to record, stream, or connect to a SIP gateway

We’re going to create a web service using python and web.py. Note that we need to install the dependencies inside the my_mediabridge container that we created in the previous step. To do that you can run bash to get to the container shell:

```bash
sudo docker exec -i -t my_mediabridge bash
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(172).png)

Now that you’re in your container, install web-py to this Docker image:

```bash
apt-get update 
apt-get install -y python python-webpy vim
```

Within the container, create a new python script:

```bash
mkdir /home/webapp
vim /home/webapp/app.py
```

### app.py

```bash
#!/usr/bin/python
import web,sys
import os
import urllib2
import base64
import binascii
from datetime import datetime
import calendar, time
import hashlib
import hmac
import sys
import random
urls = (
‘/record/(.*)’, ‘record’
)
app = web.application(urls, globals())
# Developer specific parameters copy it from your vidyo.io dashboard
# https://developer.vidyo.io/dashboard
VIDYO_IO_DEVELOPER_KEY = “your key”
VIDYO_IO_APPLICATION_ID = “your application id”
TOKEN_VALID_DURATION_SECS = 600
EPOCH_SECONDS = 62167219200
def getVidyoIOToken():
type = ‘provision’
key = VIDYO_IO_DEVELOPER_KEY
jid = “recorder@” + VIDYO_IO_APPLICATION_ID
expires = TOKEN_VALID_DURATION_SECS + EPOCH_SECONDS + int(time.mktime(datetime.now().timetuple()))
vCard = “”
def to_bytes(o):
return str(o).encode(“utf-8”)
sep = b”\0" # Separator is a NULL character
body = to_bytes(type) + sep + to_bytes(jid) + sep + to_bytes(expires) + sep + to_bytes(vCard)
mac = hmac.new(bytearray(key, ‘utf8’), msg=body, digestmod=hashlib.sha384).digest()
## Combine the body with the hex version of the mac
serialized = body + sep + binascii.hexlify(mac)
b64 = base64.b64encode(serialized)
token = b64.encode(“utf8”)
encoded_token = urllib2.quote(token)
return encoded_token;
class record:
def GET(self, roomId):
encoded_token = getVidyoIOToken()
os.system (“cp /home/webapp/config /opt/vidyo”)
with open(“/opt/vidyo/config”, “a”) as myfile:
myfile.write(“resourceId=”%s”\n” % (roomId))
myfile.write(“token=”%s”\n” % (encoded_token))
os.system (“/opt/vidyo/connect &”)
return roomId
if __name__ == ‘__main__’ :
app = web.application(urls, globals())
app.run()
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(157).png)

Create a sample config file in the webapp folder:

```bash
vim /home/webapp/config
```

### Config:

```bash
##main video/audio settings
width=1280
height=720
fps=30
kbps=2000
layout=1
maxParticipants=8
overlay=1
videoCodec=H264
audioCodec=PCM
maxCallDuration=1000
#Presentation settings
presentationAllowed=1 #0 — ignore presentations 1 — replace main video with presentation
presWidth=1280
presHeight=720
presFps=5
presKbps=300
destination=”flv:/opt/vidyo/recording.flv”
log=error
#vidyo.io connection info
host=”prod.vidyo.io”
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(174).png)

Now that we have everything set up in our container, let’s go back to the EC2 instance:

```bash
exit
```

We’ll create a new image from the Docker container we just modified and name it “simple_record_ws”:

```bash
sudo docker commit my_mediabridge simple_record_ws:0.1
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(175).png)

Finally, start your web service by calling:

```bash
sudo docker run -d -p 80:5000 — name my_vidyombws simple_record_ws:0.1 python /home/webapp/app.py 5000
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(176).png)

## Step 4. Testing Your New Web App

Go to https://developer.vidyo.io/demo and launch an instance. Note that you should be signed in to the same account you used to copy the developer key and app Id values.

By default the demo sample on the demo page joins the room named demoRoom. Try signing in from different machines so you have multiple participants on the call.

Copy your EC2 instance public IP address, open a browser, and enter the following:
http://EC2-INSTANCE-PUBLIC-IP-ADDRESS/record/demoRoom

A page will open with the text “demoRoom,” but, more importantly, you’ll notice on your video conference a message that Recorder/Streamer has joined.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(177).png)

Your recorder is working. Once every participant leaves the call, the recorder will automatically stop. You can copy the recording:

```bash
sudo docker cp my_mediabridge:/opt/vidyo/recording.flv
```

Open another terminal in the same folder where your key pair is and copy the recording from your EC2 instance to your local computer using the following command:

```bash
scp -i vidyoMB-ec2-keypair.pem ec2-user@EC2-INSTANCE-PUBLIC-IP:~/recording.flv .
```

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(178).png)

![](https://github.com/Petabytz/AWS-Projects/blob/master/Video%20Chat%20Recording/Screenshot%20(178).png)

## Conclusion :
You’ve built a simple web service that will record your vidyo.io conferences on command. One of the cool things about using the EC2 service from Amazon is that you can easily scale your service to deploy multiple containers on demand to record or stream a number of conferences in parallel. In our example we deployed one container to record one conference, but you can use Amazon’s cluster service ECS Console to manage your containers.
