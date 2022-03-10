>**Production grade lambda functions will import many 3rd party libraries, which `makes bundle size of lambda too big` which directly results in either very slow or takes forever for deployments of your lambda functions with SAM or serverless framework.**
>
>**Lambda Layers** is the solution to this problem yet many people are still now aware of it
>
>This post talks about how you can make your deployments faster by removing runtime dependencies code to layers.
>
> **Do check out the Developer and Solutions Architect POV in the end for lambda layers**.
</blockquote>

## What is Lambda Layer?

In simple words, it is a plain `.zip archive` that contains a set of dependencies, libraries and even custom runtimes that you want to be available to your lambda functions.

You can create layers using the **Lambda console, the Lambda API, AWS CloudFormation, or the AWS Serverless Application Model (AWS SAM)**

*Note* :- The contents are extracted to the `/opt` directory in the execution environment. You can include up to five layers per function, which count towards the standard Lambda deployment size limits.

## Benenfits of Lambda Layer ?

- Sharing code between multiple functions.

- Faster deployments, faster testing of lambda, smaller bundle size.

### Why do we need Lambda Layer?

- Recently, I was working on a  very simple lambda function and didn't want to set up serverless framework project or anything fancy. Just plain 3-5 lines of code, but those lines of code needed an `axios dependency` which was not available in the default node.js runtime.

- Therefore sometimes when we just need to test something out or try something simple, Lambda Layers is a great friend, super simple and very easy to set up.

### Creating Lambda function

- Lambda function is pretty simple all it does is to query external API using `axios library` and return the response.

- Github Code for [lambda](https://github.com/jatinmehrotra/Lambda-Layers-Blog)

### Creating Zip file

- In order to create a zip file, just initialize a directory with `npm init`

- Install axios library

- Create a zip file out of it

**NOTE**:- The `node_modules, package-lock.json and package.json` should reside under nodejs named folder. `nodejs` name will be used by lambda when we need to load node modules.

- [sourcecode language="text"]

      mkdir nodejs
      npm init -y
      npm install axios
      zip -r layer.zip ./



    [/sourcecode]


###  Upload and attach a layer to Lambda functions

- Upload zip file to lambda ( for more than 10 GB use s3)

![layer config](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2022/03/Screenshot-2022-03-10-at-5.49.10-PM.png)

- Attach layer to the lambda function

![upload](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2022/03/Screenshot-2022-03-10-at-5.50.08-PM.png)

![attach](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2022/03/Screenshot-2022-03-10-at-5.50.29-PM.png)


- Before using the layer there was a module error

![module-error](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2022/03/Screenshot-2022-03-10-at-5.39.04-PM.png)

- After successfully attaching layer in our function we can see the successful response

![successful response](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2022/03/Screenshot-2022-03-10-at-6.12.55-PM.png)




### Developer / Solutions Architect POV

- Lambda has [limit](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) of 50 MB (zipped, for direct upload)
250 MB (unzipped). This quota applies to all the files you upload, including layers and custom runtimes.

- **So if a zipped code or unzipped code is larger than 50 MB, 250 MB respectively then lambda layers is our friend**

- **When code needs to be shared between different lambda functions then lambda layers are our ally.**


Lambda Layers has been around 2018, still, many developers aren't using its full potential. While layers are private by default, you can share with other accounts or make a layer public. Lambda layers provide a convenient and effective way to package code libraries for sharing with Lambda functions in your account. Using layers can help reduce the size of uploaded archives and make it faster to deploy your code.


Till then, **Happy Learning!**