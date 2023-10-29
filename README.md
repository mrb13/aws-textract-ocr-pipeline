# aws-textract-ocr-pipeline
This repo is a collection of scripts that mimic &amp; extend functionality of AWS Textract Bulk Document Uploader. As of Oct'23, the Bulk Document Uploader has several limitations, including bulk download of files, that are addressed here.  

# Before we begin
This repo is a series of standalone scripts (.ps1, .bat, py) that leverage AWS Textract to submit lots of records in bulk & then derive the key text from those records. The scripts alternate between PowerShell & batch files. 
1. 

```mermaid
  flowchart  TD;
      A-->B;
      A-->id1[/This is the text in the box/];
      B-->D;
      id1-->D;
    subgraph Phase1
    s1[/batch-aws-upload.ps1/]--upload-->aws3[/AWS-S3s\]
    end
    subgraph Phase2
    aws3-->s2[/aws-textract-batch-upload.bat/]
    f1(aws-input-files.txt)-->s2
    s2-->textract1[/AWS-Textract\]
    textract1-->f2(02-JobIDs.txt)
    end
    subgraph Phase3
    f2-->s3[/aws-textract-get-documents-json.bat/]
    s3--retrieve-->textract2[/AWS-Textract\]
    textract2-->f3(JOBID.json)
    end
    subgraph Phase4
    f1-->s4[/combine-files-jobIDs.ps1/]
    f2-->s4
    f3-->s4
    s4-->f4(04-combo-output.csv)
    f4-->man[[clean-up CSV]]
    man-->s5[/rename-jobID-to-orig.ps1/]
    s5-->f5(*filename*.json)
    end
    subgraph Phase5
    f3-->c2
    end
```

# Pre-requisites 
* ran from a windows machine
* python installed
* PowerShell with AWS
* AWS command

# Phase 1: Upload to S3
Assume you have a folder that contains many files in a flattend structure (no subfolders). 
```
aws s3 sync D:\path\to\flat_folder_name  s3://<bucket-name>/
```

check your work
```cmd
aws s3 ls  s3://<bucket-name>/  > s3_count.txt
```

# Phase 2: AWS Textract - Submit Files
* get-filenames.ps1
# Phase 3: AWS Textract -Retrieve Files
# Phase 4: Clean-up Files
# Phase 5: Get Raw Text

