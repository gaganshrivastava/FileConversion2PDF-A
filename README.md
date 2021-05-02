# FileConversion2PDF-A
Server less lambda function which Convert Word/PDF files into PDF/A files.

Conversion from PDF/Word to PDF/A

Overview
This lambda function converts the word document (docx) or pdf document into a PDF/A document file. PDF/A is the standard document file which can't be edited further and can be stored for a longer period of time. Input is passed into the lambda as a JSON format. Functionality of this lambda function is divided into 4 parts.
Download the input file from S3 bucket into the local repository. 
Check the extension of the file. If it’s a word file then convert it into a pdf file.
Now, at this point all the files are in PDF format. Call the ConvertToPdfa function to convert the file into a  PDF/A file.
Once all the files have been successfully converted then upload the file into S3 bucket.


Implementation
Coding Language  : Python 3.7
Library/Tools used : VSCode, AWS Cloud9, PDFTron, AWS S3, AWS Lambda

PDFTron is an open source library which we can use to perform almost all the operations on a PDF file. It supports multiple languages like Node.js, Java, Python etc. I have used this library in merging docx/ pdf files into a single PDF.
In python it supports upto version <3.8.

Create a Lambda function, add the required roles to it from the Permission setting inside Configuration. Role likeAmazonS3FullAccess. 


After creating the lambda function setup the code in local.
Command to install the library: 
> python -m pip install boto3 -t .
> python -m pip install PDFNetPython3 -t .

To upload the code on Lambda, we have to zip all the libraries and code together. 
Command to zip the code and library into the current directory: 
> zip -r lambda_function.zip .
(Period means all)

To run and test the lambda function, cross check the runtime setting of lambda function.
In Handler the first parameter should be the file_name of the code file. Like for example my code file name is “lambda_function.py”, so lambda_function should come first. Then function name which invoked our lambda function and which takes the event and context as an input.


Input
{
  "files": [
    {
      "displayName": "CS351_HW1.pdf",
      "key": "a0aa0f2c-da69-4420-b5ea-bcd06b93ffb1/1617646540564/a0aa0f2c-da69-4420-b5ea-bcd06b93ffb116176465405641.vnd.openxmlformats-officedocument.wordprocessingml.document",
      "location": "https://unsanitized-bucket.s3.amazonaws.com//bb41fc75-4e28-4c9b-862e-fe79393aa8a2/1617215163866/bb41fc75-4e28-4c9b-862e-fe79393aa8a216172151638661.pdf"
    }
  ],
  "selectedOptions": {
    "stripMetaData": 0,
    "textExtract": 0,
    "virusScan": 0,
    "imageRecognition": 0,
    "convertDocument": 1,
    "documentClassification": 0,
    "redactPII": 0
  },
  "userID": "userID",
  "submitTime": "epsilonTime"
}



Output
{
  "statusCode": 200,
  "files": [
    {
      "displayName": "CS351_HW1.pdf",
      "key": "a0aa0f2c-da69-4420-b5ea-bcd06b93ffb1/1617646540564/a0aa0f2c-da69-4420-b5ea-bcd06b93ffb116176465405641.vnd.openxmlformats-officedocument.wordprocessingml.document",
      "location": "https://unsanitized-bucket.s3.amazonaws.com//bb41fc75-4e28-4c9b-862e-fe79393aa8a2/1617215163866/bb41fc75-4e28-4c9b-862e-fe79393aa8a216172151638661.pdf"
    }
  ],
  "selectedOptions": {
    "stripMetaData": 0,
    "textExtract": 0,
    "virusScan": 0,
    "imageRecognition": 0,
    "convertDocument": 0,
    "documentClassification": 0,
    "redactPII": 0
  }
}


Issues/Error Occurred While Developments
 
 After exploring a couple of libraries I got the PDFTron which is suitable for all use cases.The libraries which didn’t work because of errors are docx2pdf ( "errorMessage": "docx2pdf is not implemented for linux as it requires Microsoft Word to be installed"), LibreOffice (it needs an Libreoffice software to be present on the running machine)

Note: Once the file is converted into PDFA, we can’t edit it.  So the text extract function won’t work on the PDFA file.
