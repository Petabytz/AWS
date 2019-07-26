### Automatically Extract data using AWS Textract.
 Documents are a primary tool for record keeping, communication, collaboration, and transactions across many industries, including financial, medical, legal, and real estate. The millions of mortgage applications and hundreds of millions of W2 tax forms processed each year are just a few examples of such documents. A lot of information is locked in unstructured documents. It usually requires time-consuming and complex processes to enable search and discovery, business process automation, and compliance control for these documents.

In this post, I show how you can take advantage of Amazon Textract to automatically extract text and data from scanned documents without any machine learning (ML) experience. While AWS takes care of building, training, and deploying advanced ML models in a highly available and scalable environment, you take advantage of these models with simple-to-use API actions.

Text detection from documents
Multi-column detection and reading order
Natural language processing and document classification
Natural language processing for medical documents
Document translation
Search and discovery
Form extraction and processing
Compliance control with document redaction
Table extraction and processing
PDF document processing
### What do you mean by Amazon Textract..?

Amazon Textract goes beyond simple optical character recognition (OCR) to also identify the contents of fields in forms and information stored in tables. This allows you to use Amazon Textract to instantly “read” virtually any type of document and accurately extract text and data without the need for any manual effort or custom code.

### Steps to extract a Sample data:

Step 1- The following images show an example document and corresponding extracted text, form, and table data using Amazon Textract in the AWS Management Console.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*TlOYw6sgOnq6mn9Q.gif)
Step2- The following image shows the lines extracted as raw text from the document.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*xdJttnMGuxNZehT-.png)
Step 3- The following image shows the extracted form fields and their corresponding values.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*PyOSG11VNs86rpO8.gif)
Step 4- The following image shows the extracted table, cells, and the text in those cells.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*07Vq8CXTEERiE0LL.gif)
Step 5- o quickly download a zip file containing the output, choose Download results. You can choose various formats, including raw JSON, text, and CSV files for forms and tables.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*IOr0Vrx1KiNsLEXi.gif)
In addition to the detected content, Amazon Textract provides additional information, like confidence scores and bounded boxes for detected elements. It gives you control on how you consume extracted content and integrate it into various business applications.

Amazon Textract provides both synchronous and asynchronous API actions to extract document text and analyze the document text data. Synchronous APIs can be used for single page document and low latency use cases such as mobile capture. Asynchronous APIs can be used for multi page documents such as PDF documents with thousands of pages.
### Text detection from documents:
I start with a simple example on how to detect text from a document. Use the following image as an input document to Amazon Textract. As you can see, the sample image is not of good quality, but Amazon Textract can still detect the text with accuracy.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*s7w2H2nyj2ROiaK2.gif)
The following code example shows how to use a few lines of code to send this sample image to Amazon Textract and get a JSON response back. You then iterate over the blocks in JSON and print the detected text, as shown below.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./Screenshot%20from%202019-07-26%2019-54-13.png)
The following JSON response is what you receive from Amazon Textract, with blocks representing detected text in the document.

{
    "Blocks": [
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 1.0, 
                    "Top": 0.0, 
                    "Left": 0.0, 
                    "Height": 1.0
                }, 
                "Polygon": [
                    {
                        "Y": 0.0, 
                        "X": 0.0
                    }, 
                    {
                        "Y": 0.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 0.0
                    }
                ]
            }, 
            "Relationships": [
                {
                    "Type": "CHILD", 
                    "Ids": [
                        "2602b0a6-20e3-4e6e-9e46-3be57fd0844b", 
                        "82aedd57-187f-43dd-9eb1-4f312ca30042", 
                        "52be1777-53f7-42f6-a7cf-6d09bdc15a30", 
                        "7ca7caa6-00ef-4cda-b1aa-5571dfed1a7c"
                    ]
                }
            ], 
            "BlockType": "PAGE", 
            "Id": "8136b2dc-37c1-4300-a9da-6ed8b276ea97"
        }..... 
        
    ], 
    "DocumentMetadata": {
        "Pages": 1
    }
}

The following image shows the output of the detected text.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*GSigEGpQFWWSBVAT.gif)
### Natural language processing and document classification:
Customer emails, support tickets, product reviews, social media, even advertising copy all represent insights into customer sentiment that can be put to work for your business. A lot of such content contains images or scanned versions of documents. After text is extracted from these documents, you can use Amazon Comprehend to detect sentiment, entities, key phrases, syntax and topics. You can also train Amazon Comprehend to detect custom entities based on your business domain. These insights can then be used to classify documents, automate business process workflows, and ensure compliance.

The following example code shows processing the first image sample used earlier with Amazon Textract to extract text and then using Amazon Comprehend to detect sentiment and entities.

import boto3

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "simple-document-image.jpg"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.detect_document_text(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    })

#print(response)

# Print text
print("\nText\n========")
text = ""
for item in response["Blocks"]:
    if item["BlockType"] == "LINE":
        print ('\033[94m' +  item["Text"] + '\033[0m')
        text = text + " " + item["Text"]

# Amazon Comprehend client
comprehend = boto3.client('comprehend')

# Detect sentiment
sentiment =  comprehend.detect_sentiment(LanguageCode="en", Text=text)
print ("\nSentiment\n========\n{}".format(sentiment.get('Sentiment')))

# Detect entities
entities =  comprehend.detect_entities(LanguageCode="en", Text=text)
print("\nEntities\n========")
for entity in entities["Entities"]:
    print ("{}\t=>\t{}".format(entity["Type"], entity["Text"]))
    
The following image shows the output text along with the text analysis from Amazon Comprehend. You can see that it found the sentiment to be “Neutral” and detected “Amazon” as an organization, “Seattle, WA” as a location and “July 5th, 1994” as a date, along with other entities.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*IyXwHEi-CfAcvqxo.gif)
### Document translation:

Many organizations localize content for international users, such as websites and applications. They must translate large volumes of documents efficiently. You can use Amazon Textract along with Amazon Translate to extract text and data and then translate them into other languages.

The following code example shows translating the text in the first image to German.

import boto3

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "simple-document-image.jpg"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.detect_document_text(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    })

#print(response)

# Amazon Translate client
translate = boto3.client('translate')

print ('')
for item in response["Blocks"]:
    if item["BlockType"] == "LINE":
        print ('\033[94m' +  item["Text"] + '\033[0m')
        result = translate.translate_text(Text=item["Text"], SourceLanguageCode="en", TargetLanguageCode="de")
        print ('\033[92m' + result.get('TranslatedText') + '\033[0m')
    print ('')
    
The following image shows the output of the detected text, translated to German line by line.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*X8gRU9Eg3icuM5hC.gif)

### Form extraction and processing:

Amazon Textract can provide the inputs required to automatically process forms without human intervention. For example, a bank could write code to read PDFs of loan applications. The information contained in the document could be used to initiate all of the necessary background and credit checks to approve the loan so that customers can get instant results for their application rather than having to wait several days for manual review and validation.

The following image is an employment application with form fields and a table.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*2z9jKi6QaOj7DhGD.gif)
The following code example shows how to extract forms from the employment application and process different fields.
import boto3
from trp import Document

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "employmentapp.png"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.analyze_document(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    },
    FeatureTypes=["FORMS"])

#print(response)

doc = Document(response)

for page in doc.pages:
    # Print fields
    print("Fields:")
    for field in page.form.fields:
        print("Key: {}, Value: {}".format(field.key, field.value))

 # Get field by key
 print("\nGet Field by Key:")
 key = "Phone Number:"
 field = page.form.getFieldByKey(key)
 if(field):
 print("Key: {}, Value: {}".format(field.key, field.value))

 # Search fields by key
 print("\nSearch Fields:")
 key = "address"
 fields = page.form.searchFieldsByKey(key)
 for field in fields:
 print("Key: {}, Value: {}".format(field.key, field.value))
 
 The following image is the output of detected form for the employment application.
 ![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*Q3s1wR9b8NEn0TtN.gif)
 
 ### Compliance control with document redaction

Because Amazon Textract identifies data types and form labels automatically, AWS helps secure infrastructure so that you can maintain compliance with information controls. For example, an insurer could use Amazon Textract to feed a workflow that automatically redacts personally identifiable information (PII) for review before archiving claim forms. Amazon Textract recognizes the important fields that require protection.

The following code example shows extracting all the form fields in the employment application used earlier, and then redacting all the address fields.

import boto3
from trp import Document
from PIL import Image, ImageDraw

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "employmentapp.png"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.analyze_document(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    },
    FeatureTypes=["FORMS"])

#print(response)

doc = Document(response)

# Redact document
img = Image.open(documentName)

width, height = img.size

if(doc.pages):
    page = doc.pages[0]
    for field in page.form.fields:
        if(field.key and field.value and "address" in field.key.text.lower()):
        #if(field.key and field.value):
            print("Redacting => Key: {}, Value: {}".format(field.key.text, field.value.text))
            
            x1 = field.value.geometry.boundingBox.left*width
            y1 = field.value.geometry.boundingBox.top*height-2
            x2 = x1 + (field.value.geometry.boundingBox.width*width)+5
            y2 = y1 + (field.value.geometry.boundingBox.height*height)+2

            draw = ImageDraw.Draw(img)
            draw.rectangle([x1, y1, x2, y2], fill="Black")

img.save("redacted-{}".format(documentName))

The following image is the output redacted version of employment application.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*OdvYxUJIwqI9Qvjw.gif)

### Table extraction and processing

Amazon Textract can detect tables and their content. A company can extract all the amounts from an expense report and apply rules, such as any expense more than $1000 needs extra review.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*9Ptw0hLfrv3oL2jO.gif)
The following code example uses the expense report sample document and prints the content of each cell, along with a warning message if any expense is more than $1000.
import boto3
from trp import Document

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "expense.png"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.analyze_document(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    },
    FeatureTypes=["TABLES"])

#print(response)

doc = Document(response)

def isFloat(input):
  try:
    float(input)
  except ValueError:
    return False
  return True

warning = ""
for page in doc.pages:
     # Print tables
    for table in page.tables:
        for r, row in enumerate(table.rows):
            itemName  = ""
            for c, cell in enumerate(row.cells):
                print("Table[{}][{}] = {}".format(r, c, cell.text))
                if(c == 0):
                    itemName = cell.text
                elif(c == 4 and isFloat(cell.text)):
                    value = float(cell.text)
                    if(value > 1000):
                        warning += "{} is greater than $1000.".format(itemName)
if(warning):
    print("\nReview needed:\n====================\n" + warning)
    
    
The following text is the output of the table cells and the text within.
Table[0][0] = Expense Description 
Table[0][1] = Type 
Table[0][2] = Date 
Table[0][3] = Merchant Name 
Table[0][4] = Amount (USD) 
Table[1][0] = Furniture (Desks and Chairs) 
Table[1][1] = Office Supplies 
Table[1][2] = 5/10/1019 
Table[1][3] = Merchant One 
Table[1][4] = 1500.00 
Table[2][0] = Team Lunch 
Table[2][1] = Food 
Table[2][2] = 5/11/2019 
Table[2][3] = Merchant Two 
Table[2][4] = 100.00 
Table[3][0] = Team Dinner 
Table[3][1] = Food 
Table[3][2] = 5/12/2019 
Table[3][3] = Merchant Three 
Table[3][4] = 300.00 
Table[4][0] = Laptop 
Table[4][1] = Office Supplies 
Table[4][2] = 5/13/2019 
Table[4][3] = Merchant Three 
Table[4][4] = 200.00 
Table[5][0] = 
Table[5][1] = 
Table[5][2] = 
Table[5][3] = 
Table[5][4] = 
Table[6][0] = 
Table[6][1] = 
Table[6][2] = 
Table[6][3] = 
Table[6][4] = 
Table[7][0] = 
Table[7][1] = 
Table[7][2] = 
Table[7][3] = 
Table[7][4] = 
Table[8][0] = 
Table[8][1] = 
Table[8][2] = 
Table[8][3] = Total 
Table[8][4] = 2100.00 

Review needed:
====================
Furniture (Desks and Chairs) is greater than $1000.

### PDF document processing:

For the earlier examples, you used images with the sync API operations. Now, see how you can process PDF files using the async API operations.

First, use Start Document detection or Start Doument analysis to start an Amazon Textract job. As the job completes, Amazon Textract publishes the results of an Amazon Textract request, including completion status, to Amazon SNS. You can then use Get Document Text detection or Get Document Analysis to get the results from Amazon Textract.

The following code example shows how to start a job, get job status, and then process the results.

import boto3
import time

def startJob(s3BucketName, objectName):
    response = None
    client = boto3.client('textract')
    response = client.start_document_text_detection(
    DocumentLocation={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': objectName
        }
    })

    return response["JobId"]

def isJobComplete(jobId):
    # For production use cases, use SNS based notification 
    # Details at: https://docs.aws.amazon.com/textract/latest/dg/api-async.html
    time.sleep(5)
    client = boto3.client('textract')
    response = client.get_document_text_detection(JobId=jobId)
    status = response["JobStatus"]
    print("Job status: {}".format(status))

    while(status == "IN_PROGRESS"):
        time.sleep(5)
        response = client.get_document_text_detection(JobId=jobId)
        status = response["JobStatus"]
        print("Job status: {}".format(status))

    return status

def getJobResults(jobId):

    pages = []

    client = boto3.client('textract')
    response = client.get_document_text_detection(JobId=jobId)
    
    pages.append(response)
    print("Resultset page recieved: {}".format(len(pages)))
    nextToken = None
    if('NextToken' in response):
        nextToken = response['NextToken']

    while(nextToken):

        response = client.get_document_text_detection(JobId=jobId, NextToken=nextToken)

        pages.append(response)
        print("Resultset page recieved: {}".format(len(pages)))
        nextToken = None
        if('NextToken' in response):
            nextToken = response['NextToken']

    return pages

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "Amazon-Textract-Pdf.pdf"

jobId = startJob(s3BucketName, documentName)
print("Started job with id: {}".format(jobId))
if(isJobComplete(jobId)):
    response = getJobResults(jobId)

#print(response)

# Print detected text
for resultPage in response:
    for item in resultPage["Blocks"]:
        if item["BlockType"] == "LINE":
            print ('\033[94m' +  item["Text"] + '\033[0m')
            
The following image shows the job status as the API call proceeds.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Automatically%20Extract%20data%20using%20AWS%20Textract./0*nK2Ff0fX9sg7QuE9.gif)

### Conclusion:

This is how, to use Amazon Textract to automatically extract text and data from scanned documents without any machine learning (ML) experience.But there are many other opportunities where the ability to unlock text and data from unstructured documents could be most useful.




