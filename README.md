## sample-jenkins-pipeline-on-aws

### Project overview

Create and run an instance on AWS, configure Jenkins, and create a pipeline to deploy a static website on S3

Steps to follow:

* Create an IAM role with AmazonEC2FullAccess,AmazonVPCFullAccess,AmazonS3FullAccess and Launch an instance with this role attached and also create a new keypair or attach any existing keypair to connect to the instance
* Install Jenkins on Instance. For installation instructions refer link ![Jenkins setup](https://pkg.jenkins.io/redhat-stable/)
* Set up Jenkins. Visit Jenkins on its default port, 8080, with your server IP address or domain name included like this: `http://your_server_ip_or_domain:8080`
* Install required plugins(BlueOcean and pipeline-aws). BlueOcean helps in creating a pipeline by linking the github repo
* Set up GitHub with the project repository and add the Jenkinsfile.
* Set up AWS credentials in Jenkins
* Set up S3 Bucket and create a bucket policy to give required permissions to access the bucket 
* Set up pipeline for AWS by creating a stage in Jenkins file to upload the "index.html" file to s3 bucket using the region and credentials for AWS. For further info click ![withAWS](https://github.com/jenkinsci/pipeline-aws-plugin#withaws.),![s3upload](https://github.com/jenkinsci/pipeline-aws-plugin#s3upload)
    ```
    <!doctype html>
    <html>
      <head>
        <title>Static HTML Site</title>
      </head>
      <body>
        <p>This is a simple Static HTML site.</p>
      </body>
    </html>
    ```

* To prevent getting an invalid HTML, run a linter so that it fails the job if anything gets in that is invalid 
   ```
   Install the tidy linter in the server
   sudo apt-get install -y tidy
   ```
   ```
   Add stage lint with the below command
   tidy -q -e *.html
   ```
* Save, commit, and push. Within minutes, a new run should appear
* After the upload of index.html to s3 bucket enable Static website hosting. Enable the "Use this bucket to host a website" and type in "index.html" for the Index document. Click "save."
* To verify, go to the URL where the static S3 website is: http://BUCKET_NAME.s3-website.REGION.amazonaws.com/. Replace "BUCKET_NAME" with the bucket that was created early, and "REGION" with the according region where the bucket exists




