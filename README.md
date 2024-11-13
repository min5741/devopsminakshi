#  # Listed below are the objects most commonly associated with deploying and managing an application:

BuildConfigs
This is a special case resource in the context of application promotion. While a BuildConfig is certainly a part of the application, especially from a developer’s perspective, typically the BuildConfig is not promoted through the pipeline. It produces the Image that is promoted (along with other items) through the pipeline.

Templates
In terms of application promotion, Templates can serve as the starting point for setting up resources in a given staging environment, especially with the parameterization capabilities. Additional post-instantiation modifications are very conceivable though when applications move through a promotion pipeline. See Scenarios and Examples for more on this.

Routes
These are the most typical resources that differ stage to stage in the application promotion pipeline, as tests against different stages of an application access that application via its Route. Also, remember that you have options with regard to manual specification or auto-generation of host names, as well as the HTTP-level security of the Route.

Services
If reasons exist to avoid Routers and Routes at given application promotion stages (perhaps for simplicity’s sake for individual developers at early stages), an application can be accessed via the Cluster IP address and port. If used, some management of the address and port between stages could be warranted.

Endpoints
Certain application-level services (e.g., database instances in many enterprises) may not be managed by OpenShift Dedicated. If so, then creating those Endpoints yourself, along with the necessary modifications to the associated Service (omitting the selector field on the Service) are activities that are either duplicated or shared between stages (based on how you delineate your environment).

Secrets
The sensitive information encapsulated by Secrets are shared between staging environments when the corresponding entity (either a Service managed by OpenShift Dedicated or an external service managed outside of OpenShift Dedicated) the information pertains to is shared. If there are different versions of the said entity in different stages of your application promotion pipeline, it may be necessary to maintain a distinct Secret in each stage of the pipeline or to make modifications to it as it traverses through the pipeline. Also, take care that if you are storing the Secret as JSON or YAML in an SCM, some form of encryption to protect the sensitive information may be warranted.

DeploymentConfigs
This object is the primary resource for defining and scoping the environment for a given application promotion pipeline stage; it controls how your application starts up. While there are aspects of it that will be common across all the different stage, undoubtedly there will be modifications to this object as it progresses through your application promotion pipeline to reflect differences in the environments for each stage, or changes in behavior of the system to facilitate testing of the different scenarios your application must support.

ImageStreams, ImageStreamTags, and ImageStreamImage
Detailed in the Images and Image Streams sections, these objects are central to the OpenShift Dedicated additions around managing container images.

ServiceAccounts and RoleBindings
Management of permissions to other API objects within OpenShift Dedicated, as well as the external services, are intrinsic to managing your application. Similar to Secrets, the ServiceAccounts and RoleBindings objects can vary in how they are shared between the different stages of your application promotion pipeline based on your needs to share or isolate those different environments.

PersistentVolumeClaims
Relevant to stateful services like databases, how much these are shared between your different application promotion stages directly correlates to how your organization shares or isolates the copies of your application data.

ConfigMaps
A useful decoupling of Pod configuration from the Pod itself (think of an environment variable style configuration), these can either be shared by the various staging environments when consistent Pod behavior is desired. They can also be modified between stages to alter Pod behavior (usually as different aspects of the application are vetted at different stages).

Images
As noted earlier, container images are now artifacts of your application. In fact, of the new applications artifacts, images and the management of images are the key pieces with respect to application promotion. In some cases, an image might encapsulate the entirety of your application, and the application promotion flow consists solely of managing the image.

Images are not typically managed in a SCM system, just as application binaries were not in previous systems. However, just as with binaries, installable artifacts and corresponding repositories (such as RPMs, RPM repositories, or Nexus) arose with similar semantics to SCMs. Therefore, constructs and terminology around image management that are similar to SCMs have emerged:

Image registry == SCM server

Image repository == SCM repository

As images reside in registries, application promotion is concerned with ensuring the appropriate image exists in a registry that can be accessed from the environment that needs to run the application represented by that image.

Rather than reference images directly, application definitions typically abstract the reference into an image stream. This means the image stream will be another API object that makes up the application components. For more details on image streams, see Core Concepts.

Summary
Now that the application artifacts of note, images and API objects, have been detailed in the context of application promotion within OpenShift Dedicated, the notion of where you run your application in the various stages of your promotion pipeline is next the point of discussion.

Deployment Environments
A deployment environment, in this context, describes a distinct space for an application to run during a particular stage of a CI/CD pipeline. Typical environments include development, test, stage, and production, for example. The boundaries of an environment can be defined in different ways, such as:

Via labels and unique naming within a single project.

Via distinct projects within a cluster.

Via distinct clusters.

And it is conceivable that your organization leverages all three.


               # Security Best Practices for IAM ROLES WITH LEAST PRIVILEDGE ENCRYPTED STORAGE
Here are some security best practices for AWS Identity and Access Management (IAM) roles:
Least privilege
Grant users and services only the permissions they need to do their tasks. This reduces the risk of unauthorized access and accidental resource changes. 

Rotate credentials
Regularly change or rotate access keys. 

Avoid root user
The root user has full administrative privileges over the AWS account and should be avoided. 

Enforce strong passwords
Configure an account password policy that includes password rotation and discourages the use of old passwords. 

Enable MFA
Use Multi-Factor Authentication (MFA) to add an extra layer of security for interacting with the AWS API. 

Remove unnecessary credentials
Remove outdated or unnecessary credentials. For example, if a user only uses the console, remove their access keys. 

Use IAM roles
Use IAM roles for workloads and human users accessing AWS resources. This ensures they use temporary credentials. 

                # Ways to optimise cost like auto scaling for non production enviornment
Here are some ways to optimize costs for a non-production environment:
Auto scaling
Automatically adds resources when needed, reducing the cost of keeping resources online permanently. 

Spot instances
Bid for unused cloud capacity at a discount, which can be a cost-effective option for non-critical workloads. 

Pricing model
Choose a pricing model that fits your budget and workload, such as pay-as-you-go, Reserved Instances, or Spot Instances. 

Load balancing
Route traffic to the nearest data center or cloud region based on the user's location, which can reduce latency and improve performance. 

Savings plans
A flexible pricing model that offers lower prices in exchange for commitment
