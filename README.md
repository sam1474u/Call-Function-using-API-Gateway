# Call-Function-using-API-Gateway
In this tutorial we will take a look at one of the newest offerings in the Oracle Cloud - How to call a Function using API Gateway.

In this tutorial we will take a look at one of the newest offerings in the Oracle Cloud - API Gateway.

We'll do the following things:

Create and deploy a "hello world" serverless function
Create a subnet suitable for our API gateway
Create a dynamic group and apply the necessary policies for API gateway
Create the gateway
Deploy a spec to the gateway
Test the gateway

Create and deploy a "hello world" serverless function


We need a Oracle Cloud Infrastructure account to set up Oracle Functions development using Cloud Shell. Then, we create a function application and a function. Key tasks include how to:

Set up an authentication token.
Gather required information.
Set up a VCN.
Log in to OCI Registry (OCIR).
Configure Cloud Shell to deploy functions.
Configure your Fn context.
Create an app for your Oracle function.
Create a function.
Deploy your function.
Test your function.

![image](https://user-images.githubusercontent.com/42166489/107554602-329cce80-6bfc-11eb-8c3a-7ea319648478.png)

For additional information, please see:

Start for Free : https://www.oracle.com/cloud/free/
Launch your first Linux VM: https://docs.oracle.com/iaas/Content/GSG/Reference/overviewworkflow.htm
Cloud Shell: https://docs.oracle.com/iaas/Content/API/Concepts/cloudshellintro.htm
Oracle Functions: https://docs.oracle.com/iaas/Content/Functions/Concepts/functionsoverview.htm
OCI Registry: https://docs.oracle.com/iaas/Content/Registry/Concepts/registryoverview.htm

Before You Begin

To successfully perform this tutorial, we must have the following:

A paid Oracle Cloud Infrastructure account. See Signing Up for Oracle Cloud Infrastructure.
Link- https://docs.oracle.com/iaas/Content/GSG/Tasks/signingup.htm
Your OCI account configured to support Oracle Functions development. See Create Policy for Oracle Functions.
https://www.oracle.com/webfolder/technetwork/tutorials/infographics/oci_functions_cloudshell_quickview/functions_quickview_top/functions_quickview/index.html#
OCI Cloud Shell which is included with your account and includes:

OCI CLI
Python 3.6+
Java 1.8+
Node.js 10+

1.Gather Required Information

Collect all the information needed to complete the tutorial.

Gather Region and Registry Information
Prepare the information we need from the Oracle Cloud Infrastructure Console.

Find your region identifier and region key from Regions and Availability Domains.
Example: us-ashburn-1 and iad for Ashburn.
https://docs.oracle.com/iaas/Content/General/Concepts/regions.htm

Create a registry project name to store our function images in OCI Registry (OCIR).
When we publish a function, a Docker image is created in OCIR. Your OCIR project name is prepended to our function images to make them easy to find. For example, given:

Registry project name: my-func-prj
Function name: node-func
Your function image would be stored on OCIR under: my-func-prj/node-func

Get Compartment, Auth Token, and Collect your Data

Create or Select a Compartment
https://docs.oracle.com/iaas/Content/Identity/Tasks/managingcompartments.htm#To

To create a compartment see Create a compartment. After our compartment is created, save the compartment OCID.

To get the compartment OCID from an existing compartment:

Go to Identity then Compartments.
Select your compartment.
Click the Copy link for the OCID field.

![image](https://user-images.githubusercontent.com/42166489/107554888-890a0d00-6bfc-11eb-8fb6-5077d126d1a2.png)

Compartment is Created:

![image](https://user-images.githubusercontent.com/42166489/107554910-8d362a80-6bfc-11eb-8765-6d80f045085f.png)

Create an Authorization Token
We create an authorization token to log in to the OCI Registry. To create an authorization token:

In the top navigation bar, click your user avatar.
Select your username.
Click Auth Tokens.
Click Generate Token.
Give it a description.
Click Generate Token.
Copy the token and save it.

![image](https://user-images.githubusercontent.com/42166489/107554926-93c4a200-6bfc-11eb-882f-0740542f34f2.png)

Collect our Information

Collect all the information needed to complete the tutorial. Copy the following information into your notepad.

Region: <region-identifier>
Example: ap-mumbai-1.

Region Key: <region-key>
Example: bom.

Registry Project Name: <your-project-name>
Example: my-func-prj.

Compartment ID: <compartment-id>
Example: ocid1.compartment.oc1..aaaaaaaa2nmz2tuisdthmpeisu6ubjsenbvabvuzxigxst4vbzrnsp7i4csq

Auth Token: <auth-token>
Example: 3:0y1BF2Jj+;s_<sn]fE

Tenancy name: <tenancy-name>
From your user avatar, example: tcsbangalore2

Tenancy OCID: <tenancy-ocid>
From your user avatar, go to Tenancy: <your-tenancy> and copy OCID, example: ocid1.tenancy.oc1..aaaaaaaaonu3r4bfkxpkccywutzd4ekmzt3ys4krlrojg25ixuwd3oxhliya

Username: <user-name>
From your user avatar.
Eg: oracleidentitycloudservice/Saikat

Note: For a user with admin access the username would be like: saikatdey

2.Create your Virtual Cloud Network (VCN)

1. From the main landing page, select Set up a network with a wizard.

![image](https://user-images.githubusercontent.com/42166489/107554957-9b844680-6bfc-11eb-9758-9569d7970e18.png)

2. In the Start VCN Wizard workflow, select VCN with Internet Connectivity and then click Start VCN Wizard .
3. In the configuration dialog, fill in the VCN Name for our VCN. Our Compartment is already set to its default value of <our-tenancy> (root).
4. In the Configure VCN and Subnets section, keep the default values for the CIDR blocks:
VCN CIDR BLOCK: 10.0.0.0/16
PUBLIC SUBNET CIDR BLOCK: 10.0.0.0/24
PRIVATE SUBNET CIDR BLOCK: 10.0.1.0/24

![image](https://user-images.githubusercontent.com/42166489/107554987-a6d77200-6bfc-11eb-829c-a72782a110b4.png)

![image](https://user-images.githubusercontent.com/42166489/107551655-8a393b00-6bf8-11eb-8173-82513a568c50.png)

5. For DNS RESOLUTION uncheck USE DNS HOSTNAMES IN THIS VCN.Picture shows the USE DNS HOSTNAMES IN THIS VCN option is unchecked.
Click Next.
6. The Create a VCN with Internet Connectivity configuration dialog is displayed (not shown here) confirming all the values our just entered.

![image](https://user-images.githubusercontent.com/42166489/107551781-b05edb00-6bf8-11eb-85db-3f910e123d3d.png)

7. Click Create to create our VCN.
The Creating Resources dialog is displayed (not shown here) showing all VCN components being created.

![image](https://user-images.githubusercontent.com/42166489/107551816-bbb20680-6bf8-11eb-91b2-655e369b689c.png)

8. Click View Virtual Cloud Network to view our new VCN.
Our new VCN is displayed. Now we need to add a security rule to allow HTTP connections on port 3000, the default port for our application.

9. With our new VCN displayed, click our Public subnet link.
The public subnet information is displayed with the Security Lists at the bottom of the page. There should be a link to the Default Security List for our VCN.

![image](https://user-images.githubusercontent.com/42166489/107551846-c66c9b80-6bf8-11eb-8052-9d337da434c2.png)

10. Click on the Default Security List link.
The default Ingress Rules for our VCN are displayed.

We will be modifying the ingress rules later in the tutorial.


3. Configure Functions

To use Oracle Functions, we must configure the Fn application context. The context stores the values needed to connect to the Oracle Functions service. Fn client commands are used to add the required configuration data.

Configure the Fn Context for the Cloud Shell
We need the information you gathered from earlier on. Use Fn client commands to configure Fn.

Open your Cloud Shell instance.
Get a list of Fn contexts.
fn list context

We see contexts for default and <your-region-identifier>.

Select the context named with <your-region-identifier>.
For example: fn use context us-phoenix-1

List the Fn contexts to ensure <our-region-identifier> is selected. (Has a star next to it.)
Set the compartment for Oracle Functions.
Example: fn update context oracle.compartment-id ocid1.compartment.oc1..aaaaaaaarvdfa72n...

Set the URL for our Registry repository.
Sample command: fn update context registry <region-key>.ocir.io/<tenancy-namespace>/<registry-project-name>

Example: fn update context registry phx.ocir.io/my-tenancy/my-func-prj

![image](https://user-images.githubusercontent.com/42166489/107551897-d7b5a800-6bf8-11eb-8d32-834a86cac73d.png)

Note: View/Edit your Context

Our Fn context files are located in the ~/.fn/contexts directory. Each context is stored in a .yaml file. For example, our us-phoenix-1.yaml file might look similar to:

api-url: https://functions.us-phoenix-1.oci.oraclecloud.com
oracle.compartment-id: ocid1.compartment.oc1..aaaaaaaarvdfa72n...
provider: oraclecs
registry: phx.ocir.io/my-tenancy/my-func-prj
                
We can edit the file directly with an editor if necessary.
For a detailed explanation of each step, see: 
https://www.oracle.com/webfolder/technetwork/tutorials/infographics/oci_functions_cloudshell_quickview/functions_quickview_top/functions_quickview/index.html#

You have now setup the Fn context for your instance.

4. Create and Deploy a Function

With your configuration complete, it is time to create and deploy a function.

Create an Application
An Application is the main storage container for functions. Each function must have an application for deployment. To create application, follow these steps.

Open Developer Services and then Functions which are part of the Solutions and Platform grouping.
Click Create Application.
Fill in the form data.

Name: <your-app-name>
VCN: <your-VCN>
Subnets: <your-public-subnet> or <your-private-subnet>
 
Note: A public or private subnet may be used, select one.
Click Create.
Our app is created.


![image](https://user-images.githubusercontent.com/42166489/107552398-780bcc80-6bf9-11eb-842d-be1852903f52.png)

![image](https://user-images.githubusercontent.com/42166489/107552411-7e9a4400-6bf9-11eb-803c-226454ddbd52.png)

Choose a Language to Create and Deploy a Function

Select one of the following languages to create and deploy a function. If you want, you can do all three.

Select one of the following languages to create and deploy a function. If you want, you can do all three.

Create and Deploy a Java Function
With your application created, deploy a Java function. Follow these steps to create a Java "Hello World" function.

Note: Ensure Java 8+ is installed to perform these steps.
Open Cloud Shell.

Create a directory to store your functions and change into that directory.
mkdir my-dir-name
cd my-dir-name                        
                    
Create a Java "Hello World" function with Fn.

fn init --runtime java my-func-name
This command creates a directory named my-func-name with several files in it.

func.yaml - Function configuration file.
pom.xml - Maven build file.
src/main/java/com/example/fn/HelloFunction.java - The actual function file.
Change into the directory.
Deploy the function.

fn -v deploy --app your-app-name
Various messages are displayed as the docker images are built, pushed to OCIR, and eventually deployed to Oracle Functions.

Invoke the function.
fn invoke your-app-name my-func-name

Returns: Hello, world!

Invoke the function with a parameter.
echo -n "Bob" | fn invoke your-app-name my-func-name
Returns: Hello, Bob!

![image](https://user-images.githubusercontent.com/42166489/107552460-8e198d00-6bf9-11eb-8b6f-45bd0c713a29.png)

![image](https://user-images.githubusercontent.com/42166489/107552489-983b8b80-6bf9-11eb-8ac3-64f0fb311f34.png)

If you want to connect to your function from the net, you need to get the function's invoke endpoint. To find our invoke endpoint use the inspect command.

fn inspect function your-app-name my-func-name

Examine the results of the inspect command. Notice the invoke endpoint URL is included in the annotatins section of the returned JSON data.
{
    "annotations": {
        "fnproject.io/fn/invokeEndpoint": "https://aaaaaaaaa.us-ashburn-1.functions.oci.oraclecloud.com/1111111/functions/ocid1.fnfunc.oc1.iad.aaaaaaaaa.../actions/invoke",
        "oracle.com/oci/compartmentId": "ocid1.compartment.oc1..aaaaaaaa...",
        "__comment":"Remaining output left out for brevity",
Use the URL returned from inspect to invoke the function. Because functions require requests to be digitally signed, the oci raw-request command is used for this example.
oci raw-request --http-method POST --request-body "" --target-uri https://https://aaaaaaaaa.us-ashburn-1.functions.oci.oraclecloud.com/1111111/functions/ocid1.fnfunc.oc1.iad.aaaaaaaaa.../actions/invoke

The command returns:

{
    "data": "Hello, world!",
    "headers": {
        "Content-Length": "13",
        "Content-Type": "text/plain",
        "Date": "Tue, 20 Oct 2020 00:53:08 GMT",
        "Fn-Call-Id": "11111111111",
        "Fn-Fdk-Version": "fdk-java/1.0.111 (jvm=OpenJDK 64-Bit Server VM, jvmv=11.0.8)",
        "Opc-Request-Id": "1111111/11111"
    },
    "status": "200 OK"
}
 
Note: We can connect to a Functions endpoint using tools like curl. However, because of security considerations, the script is complex. For details and an example, see the oci-curl section on the Invoking Functions page.

![image](https://user-images.githubusercontent.com/42166489/107552574-b1443c80-6bf9-11eb-9296-4a955a504d10.png)

![image](https://user-images.githubusercontent.com/42166489/107552589-b6a18700-6bf9-11eb-9e44-11dbdb27ea5f.png)

We have successfully deployed and tested a Java function.

We can invoke this function with the fn CLI at this point but we can't invoke the function directly via HTTP(s) without signing the request or using the OCI SDK. 

Trying to invoke will end up returning a 401 Unauthorized:

Saikat_Dey@cloudshell:helloFunction (ap-mumbai-1)$ curl -i -X GET https://odu37kimtfq.ap-mumbai-1.functions.oci.oraclecloud.com/20181201/functions/ocid1.fnfunc.oc1.ap-mumbai-1.aaaaaaaaacdqlyex2kc64rcmi6c4ubhvho6tgxjvgc4ii4fywkmeliexn5nq/actions/invoke

![image](https://user-images.githubusercontent.com/42166489/107552642-c7ea9380-6bf9-11eb-95a2-e99198aa379d.png)

But once we put our serverless function behind our gateway we can invoke it via HTTPS. 

That is what we will try to do now.

5.Setting up the API Gateway.

Create a subnet suitable for our API gateway

We'll need a regional subnet for our API gateway that has an ingress rule for HTTPS traffic, so add one now to our existing VCN subnets. 

Now edit the chosen security list to add in ingress rule for port 443:

![image](https://user-images.githubusercontent.com/42166489/107552697-dcc72700-6bf9-11eb-8af8-dcababa0fb5e.png)

Add a ingress rule:

![image](https://user-images.githubusercontent.com/42166489/107552738-e8b2e900-6bf9-11eb-9760-04b8378b4eed.png)

Ingress rule added to SL.

Create a dynamic group and apply the necessary policies for API gateway

The API gateway uses dynamic groups to manage access in your tenancy so we will need to create a new dynamic group and set some policies. We'll need the compartment OCID for the compartment that we're going to create our gateway within, so hit Identity -> Compartments and copy the OCID that you are planning to use. Next, create a new dynamic group with the following definition (substituting the proper compartment OCID):

![image](https://user-images.githubusercontent.com/42166489/107552806-fe281300-6bf9-11eb-92e1-c37923c2b0cd.png)

Click on “Rule Builder” link to create a rule for the compartment.
Add the compartment id in the value field.



![image](https://user-images.githubusercontent.com/42166489/107553318-a5a54580-6bfa-11eb-8adb-b7a4dd06a2a7.png)

Once done press “Add Rule”. The required rule is added.

![image](https://user-images.githubusercontent.com/42166489/107553368-b2299e00-6bfa-11eb-8266-1b5c93929283.png)

Rule: 
All {instance.compartment.id = 'ocid1.compartment.oc1..aaaaaaaa2nmz2tuisdthmpeisu6ubjsenbvabvuzxigxst4vbzrnsp7i4csq'}

Create a Policy:
Now we'll need to create a policy that is specific to your tenancy and the newly created dynamic group. You'll need to substitute your own group name and compartment name as appropriate:

![image](https://user-images.githubusercontent.com/42166489/107553419-c1105080-6bfa-11eb-9b31-1d3d036defd6.png)

Now we'll need to create a policy that is specific to your tenancy and the newly created dynamic group. You'll need to substitute your own group name and compartment name as appropriate:

![image](https://user-images.githubusercontent.com/42166489/107553457-cb324f00-6bfa-11eb-863a-204224533471.png)

Click on “Create Policy”.

Eg: allow dynamic-group [your dynamic group] to use functions-family in compartment [your compartment name]

![image](https://user-images.githubusercontent.com/42166489/107553494-d6857a80-6bfa-11eb-8bda-68bb54732679.png)

Click on “Customize” and enter the commands.

![image](https://user-images.githubusercontent.com/42166489/107553540-e1d8a600-6bfa-11eb-9e72-08cac106572b.png)

Next add another policy statement:

ALLOW any-user to use functions-family in compartment <functions-compartment-name> where ALL {request.principal.type= 'ApiGateway', request.resource.compartment.id = '<api-gateway-compartment-OCID>'}

Eg: 
ALLOW any-user to use functions-family in compartment workshops where ALL {request.principal.type= 'ApiGateway', request.resource.compartment.id = 'ocid1.compartment.oc1..aaaaaaaa2nmz2tuisdthmpeisu6ubjsenbvabvuzxigxst4vbzrnsp7i4csq'}

![image](https://user-images.githubusercontent.com/42166489/107553598-f3ba4900-6bfa-11eb-8c28-0809f03de34a.png)

Policy is added.

![image](https://user-images.githubusercontent.com/42166489/107553639-fae15700-6bfa-11eb-8c88-ffcf55ca4c6b.png)

Update: Now we can also skip the dynamic group creation by changing your policy definition like so:

Now, let's create the gateway!

Create the gateway

To create the gateway, first select 'API Gateway' under 'Developer Services' in the sidebar menu:

![image](https://user-images.githubusercontent.com/42166489/107553727-0f255400-6bfb-11eb-8bfc-2d4184536230.png)

Click 'Create Gateway' and populate the dialog, making sure to choose the regional subnet that we created earlier.

![image](https://user-images.githubusercontent.com/42166489/107553782-1e0c0680-6bfb-11eb-84b7-5cc699bde8d7.png)

Once done the Gateway will be created.

![image](https://user-images.githubusercontent.com/42166489/107553827-2a905f00-6bfb-11eb-9416-468c036fe197.png)

Now there are two ways to create a Deployment.
1. Deploying a Spec
2.Creating from Scratch

1. Deploy a spec to the gateway

Before we can create a deployment we will need to craft a deployment spec file in JSON format to define our endpoints. Make sure you have the function OCID from above handy. Now, create a file called spec.json in the root of your function and populate it as follows (substitute your function OCID):

{
 "routes": [
  {
   "path": "/hellofunction",
   "methods": [
    "GET"
   ],
   "backend": {
    "type": "ORACLE_FUNCTIONS_BACKEND",
    "functionId": "ocid1.fnfunc.oc1.ap-mumbai-1.aaaaaaaaacdqlyex2kc64rcmi6c4ubhvho6tgxjvgc4ii4fywkmeliexn5nq"
   }
  }
 ]
}
Click on “Create Deployment”

![image](https://user-images.githubusercontent.com/42166489/107553908-3da32f00-6bfb-11eb-9e03-ba3532889c74.png)

![image](https://user-images.githubusercontent.com/42166489/107553937-46940080-6bfb-11eb-8577-f895cc647712.png)

![image](https://user-images.githubusercontent.com/42166489/107553970-4dbb0e80-6bfb-11eb-9ff6-dde47ff6c17f.png)

Click on Create.

1. Deploy From Scratch

![image](https://user-images.githubusercontent.com/42166489/107554173-98d52180-6bfb-11eb-80a0-f4dcaaf85b04.png)

![image](https://user-images.githubusercontent.com/42166489/107554182-9d99d580-6bfb-11eb-8c52-be4c795547fb.png)

![image](https://user-images.githubusercontent.com/42166489/107554239-b0aca580-6bfb-11eb-99e1-48a4f7a4efe6.png)

The deployment is created.

Test the gateway

On the gateway details page, take note of the endpoint.

![image](https://user-images.githubusercontent.com/42166489/107554295-c3bf7580-6bfb-11eb-97eb-9f21eadc4cc0.png)

To test our function, copy the endpoint and append the path that with a GET request.

We have created two deployments in two different ways. Now we will test both the deployments.

![image](https://user-images.githubusercontent.com/42166489/107554333-d043ce00-6bfb-11eb-8a08-069f1b96af1f.png)

Lets test “deploy-api”:

curl -i -X GET https://dq6ovcfzxrgmgpuegmtkij6bly.apigateway.ap-mumbai-1.oci.customer-oci.com/fn/hellofunction

![image](https://user-images.githubusercontent.com/42166489/107554366-dafe6300-6bfb-11eb-8fdf-c1f459af5058.png)

Lets test “api-deployment”:
curl -i -X GET https://dq6ovcfzxrgmgpuegmtkij6bly.apigateway.ap-mumbai-1.oci.customer-oci.com/v1/hellofunction

![image](https://user-images.githubusercontent.com/42166489/107554403-e6ea2500-6bfb-11eb-8a49-1c5e20bd897a.png)

We can see the out put now.  “Hello, world!”

The url is now a public URL hence can be tested on a browser as well.

![image](https://user-images.githubusercontent.com/42166489/107554431-efdaf680-6bfb-11eb-87d5-451cb07d1f82.png)

This completes our tutorial.









