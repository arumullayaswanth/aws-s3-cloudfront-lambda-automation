# End-to-End Static Website Hosting on AWS S3 with CloudFront CDN and Automated Cache Invalidation via Lambda

<table style="width: 100%; margin-bottom: 20px;">
  <tr>
    <td align="center" style="padding: 10px; background-color: #e9f7f5; border-radius: 8px;">
      <img src="https://github.com/arumullayaswanth/aws-s3-cloudfront-lambda-automation/blob/a102c7b8f2b1359642112b9dc1521b87207782fa/CloudFront%20architecture/CloudFront.png" width="1000%" style="border: 2px solid #ddd; border-radius: 10px;">
      <br><b>AWS CloudFront architecture Project </b>
    </td>
  </tr>
</table>




# BookMyShow Clone 🎫

It is an clone website of BookMyShow that developed just using HTML, CSS and JavaScript. BookMyShow is a webiste used to buy Movie tickets in online.

# Build with ⚒️

-> **HTML** | **CSS** | **JavaScript** |

# How to run Locally 🖥️

1. Before the following steps make sure you have [git](https://git-scm.com/downloads) installed on your system.

2. Clone the GitHub project in your local directory with command `git clone https://github.com/vu3tpz/BookMyShow-Clone` or you can just download the code and unzip it.


```
git clone https://github.com/arumullayaswanth/aws-s3-cloudfront-lambda-automation.git
```


3. Open index.html file in any Browser.

# Screen Layout 🎟️

![Screenshot (41)](https://user-images.githubusercontent.com/101320198/192150986-a88e788a-135c-4660-b047-7e699bdbf7b6.png)

![Screenshot (42)](https://user-images.githubusercontent.com/101320198/192151023-586af7b8-1088-401d-948f-53b99d43f930.png)

![Screenshot (43)](https://user-images.githubusercontent.com/101320198/192150619-ec0dea40-841a-4476-8b02-1124f54d4ea4.png)

![Screenshot (44)](https://user-images.githubusercontent.com/101320198/192151026-542ff47d-f0b9-4189-b3b8-a206165c5565.png)


## The End..⚔️



Based on the provided source, a CloudFront project was discussed which aimed to improve latency and overcome latency issues by configuring a CloudFront application.
Key aspects of this project as described in the source include:
•
Utilization of AWS Edge Locations: The project leveraged AWS edge locations offered by CloudFront. The concept is to keep content nearby these edge locations. When creating a CloudFront distribution, you have options to select whether to store content in specific edge locations or all edge locations. By selecting all edge locations, content is stored near many points globally, allowing clients to access it from nearby locations, which directly improves latencies.
•
Addressing Content Updates and TTL: The source mentions the challenge related to Time To Live (TTL) settings. If developers make changes to the source code, users might still access the old data for a period determined by the TTL (e.g., one hour). To ensure users immediately get the updated content, a mechanism is needed to refresh the edge locations.
•
Implementing Invalidations via Lambda Function: To address the content update issue, the project implemented a lambda function. This function is designed to perform invalidations. Invalidations are crucial because they update the new content into the edge locations instead of the old content immediately after developers update the source code.
•
Event-Driven Trigger: The lambda function is triggered by an event rule. This rule is set up to run the lambda function automatically whenever developers put data into a specific bucket. Once triggered, the lambda function runs the invalidations and updates the new content to the edge locations.
This particular project used the CloudFront configuration with edge locations and an automated invalidation process via a lambda function triggered by bucket updates to ensure low latency and prompt content delivery.
