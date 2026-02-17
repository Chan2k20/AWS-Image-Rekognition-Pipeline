# AWS Image Rekognition Pipeline

A distributed image processing pipeline built on AWS that leverages EC2, S3, SQS, and Rekognition services to perform object detection and optical character recognition (OCR) on images in parallel.

## Overview

This project demonstrates a scalable, concurrent image processing system using AWS services. The pipeline identifies cars in images with high confidence and extracts text from those images, showcasing real-world distributed computing patterns.

## Architecture

### Components

- **EC2 Instance A**: Producer service for object detection
  - Retrieves images from S3 bucket
  - Detects objects using AWS Rekognition
  - Filters images containing cars (confidence > 90%)
  - Enqueues qualified image indices to SQS

- **EC2 Instance B**: Consumer service for text recognition
  - Monitors SQS queue for incoming image indices
  - Downloads images from S3
  - Performs OCR using AWS Rekognition
  - Generates comprehensive output report

- **AWS S3**: Image storage and management
- **AWS SQS**: Asynchronous message queue for inter-instance communication
- **AWS Rekognition**: Computer vision service for object and text detection

## Key Features

✅ **Parallel Processing**: Independent concurrent execution on two EC2 instances  
✅ **Loose Coupling**: Message queue-based communication between instances  
✅ **Signaling Mechanism**: Completion indicator (-1 index) for graceful shutdown  
✅ **Flexible Execution**: Works regardless of which instance starts first  
✅ **Comprehensive Output**: Consolidated results in structured format  

## Technologies Used

- **Languages**: Java
- **Cloud Platform**: Amazon Web Services (AWS)
  - EC2 (Elastic Compute Cloud)
  - S3 (Simple Storage Service)
  - SQS (Simple Queue Service)
  - Rekognition (Computer Vision)
- **OS**: Amazon Linux AMI
- **Storage**: EBS (Elastic Block Store)

## Project Workflow

1. **Initialization**: EC2 A starts and reads images from S3 bucket (`njit-cs-643`)
2. **Object Detection**: Rekognition analyzes images for object presence
3. **Filtering**: Images with detected cars (confidence ≥ 90%) are flagged
4. **Queueing**: Image indices are sent to SQS queue
5. **Text Recognition**: EC2 B retrieves indices from queue and downloads images
6. **OCR Processing**: Rekognition extracts text from each image
7. **Output Generation**: Results written to `output.txt` containing:
   - Image indices with both cars and text detected
   - Extracted text content from each image

## Output

The pipeline generates an `output.txt` file containing:
- Indices of images containing both detected cars and recognizable text
- Extracted text from each qualified image

## Configuration

### Prerequisites
- AWS Account with appropriate IAM permissions
- EC2 instances running Amazon Linux AMI
- S3 bucket with sample images
- SQS queue for inter-instance messaging

### Environment Setup
Configure the following parameters:
- S3 bucket name: `njit-cs-643`
- Rekognition confidence threshold: 90%
- Output file location: EBS attached to EC2 B

## Performance Considerations

- **Parallelization**: Dual EC2 instances process images concurrently
- **Queue Management**: SQS handles asynchronous communication and load distribution
- **Scalability**: Architecture supports scaling with additional consumer instances


## References

- [AWS Rekognition Documentation](https://docs.aws.amazon.com/rekognition/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS SQS Documentation](https://docs.aws.amazon.com/sqs/)
