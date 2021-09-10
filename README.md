
# Cloud Native Team - Kubernetes exercise for Automation Engineer
## Project Scope

We need to run a web application that manages some polls in a <span style="color:blue">highly available infrastructure</span>.
## Architecture Design

The software architect made a choice to run the application in a <span style="color:blue">Kubernetes environment</span>, leveraging  <span style="color:blue">Django as the web application</span> framework and <span style="color:blue">PostgreSQL as the backend relational database</span>. 
The infrastructure should be monitored using <span style="color:blue">Prometheus.</span>

##Frontend

The frontend layer should run a Django stateless web application spread among different instances to grant high availability.

The web application to run is the  <span style="color:blue">Django Polls App.</span>

The web application should expose two contexts, the polls page (like http://HOST:PORT/) and the admin page (like http://HOST:PORT/admin/). Polls are managed via the admin page and votes can be added via the polls page.
Backend

The backend layer should run PostgreSQL in a High Availability configuration. The software architect selected the Cloud Native PostgreSQL Kubernetes operator as it will both enhance and simplify the management of the PostgreSQL workload.

## Monitoring

Prometheus should be running inside the cluster. It is sufficient to gather the default metrics exported by the Cloud Native PostgreSQL pods.
## Exercise Task Description

The candidate should create a Proof of Concept of the final environment that can run on Kind or on a public cloud among AKS, EKS or GKE.

The solution could be an archive or a GitHub (or equivalent) repo, with a document that describes the work that has been done and how it can be reproduced/tested, plus all the other resources that have been used/created.

The solution should contain:

    Any script (Bash or equivalent) or any automation or Step-by-Step instruction that has been used to create the POC environment;
    The Dockerfile that you wrote to create the container image that runs the web application;
    Any YAML that defines the needed Kubernetes resources in a declarative way;
    Any code that have been wrote or any existing code that have been changed to be able to run the POC;
    The instructions and resources/code to expose the web application at least on localhost (accessible via a local web browser);
    A document stating the reasons behind any meaningful decision taken by the candidate while developing the POC.

##Tips

Take a look at the following tips that will expedite your work:
###Kind

    You can expose a Service type NodePort that uses a specific TCP port from Kind to your local OS by creating the Kind cluster with a kind-config.yaml that contains the following code:
```yaml
nodes:
- role: control-plane
  extraPortMappings:
    - containerPort: 30950
      hostPort: 30950
      listenAddress: "127.0.0.1"
      protocol: TCP
- role: worker
- role: worker
- role: worker
```
###Frontend

    You can find a Django Polls App ready to be used in the DigitalOcean Community GitHub repo. It uses the MIT License.

    You have to update the Django mysite/settings.py in the DATABASES section to use the PostgreSQL engine like described in the documentation

    If you want the web application to answer at the root path of your URL, you can patch the mysite/urls.py like this: path('', include('polls.urls')),.

    The Django application must initialize the database at least once like described here.

    The Django application can be started with the command python manage.py runserver 0.0.0.0:PORT.

    This Django Polls App needs at least an admin user to be able to create/update/delete polls, and it is usually done via an interactive console like this. You can use the following code to script the user creation:

    from django.contrib.auth import get_user_model

    User = get_user_model()
    User.objects.filter(email='admin@example.com').delete()
    User.objects.create_superuser('admin', 'admin@example.com', 'admin')

    The command that can execute it could be like:

    echo "from django.contrib.auth import get_user_model; User = get_user_model(); User.objects.create_superuser('admin', 'admin@example.com', 'admin');" | python manage.py shell

    Obviously it is a quick and dirty way to have at least an admin user that SHOULD be changed after the POC has been evaluated.

###Monitoring

    Each CNP pod exports a few default metrics. If Prometheus correctly collects them, you'll see metrics starting with cnp_ available.
