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

![image](https://user-images.githubusercontent.com/42166489/107551365-2f074880-6bf8-11eb-96e8-1fc1d8e28c9e.png)

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

![image](https://user-images.githubusercontent.com/42166489/107551469-5100cb00-6bf8-11eb-8767-b77ba3bd98a4.png)

Compartment is Created:

![image](https://user-images.githubusercontent.com/42166489/107551495-59f19c80-6bf8-11eb-9b48-baec3149026a.png)

Create an Authorization Token
We create an authorization token to log in to the OCI Registry. To create an authorization token:

In the top navigation bar, click your user avatar.
Select your username.
Click Auth Tokens.
Click Generate Token.
Give it a description.
Click Generate Token.
Copy the token and save it.

![image](https://user-images.githubusercontent.com/42166489/107551536-683fb880-6bf8-11eb-8a22-8964e2aab370.png)

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

![image](https://user-images.githubusercontent.com/42166489/107551584-75f53e00-6bf8-11eb-9401-0e97a66969f5.png)

2. In the Start VCN Wizard workflow, select VCN with Internet Connectivity and then click Start VCN Wizard .
3. In the configuration dialog, fill in the VCN Name for our VCN. Our Compartment is already set to its default value of <our-tenancy> (root).
4. In the Configure VCN and Subnets section, keep the default values for the CIDR blocks:
VCN CIDR BLOCK: 10.0.0.0/16
PUBLIC SUBNET CIDR BLOCK: 10.0.0.0/24
PRIVATE SUBNET CIDR BLOCK: 10.0.1.0/24

![image](https://user-images.githubusercontent.com/42166489/107551624-84dbf080-6bf8-11eb-9d73-16aae66590e6.png)

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






