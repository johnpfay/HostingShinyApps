# Hosting Shiny Apps
**Objective**: 

* To host Henry's Shiny App on Duke's OpenShift platform

**Workflow**: 

* Create a simple shiny app
* Host that app successfully on OpenShift 
* Revise the app to a more complex one
* Ensure that it still works successfully on the server
* Revise to Henry's Shiny App, and check

**Sources**

 * Duke's cloud registry: https://cloud.duke.edu/projects
 * Duke Openshift Documentation: https://okd-docs.cloud.duke.edu/
 * Duke Repo on publishing: https://github.com/Duke-GCB/openshift-shiny

---

## Task 1: Create a simple shiny app

* Create new RStudio Project 
* Copy over folders from https://github.com/Duke-GCB/openshift-shiny":
  * `dockerfiles`
  * `examples`: Includes 3 example dashboards
  * `openshift`

## Task 2: Host the app

* Open https://cloud.duke.edu/projects

* Create new project:

  * Name: `arctest`
  * Fundcode: 
  * Platform: `ODF Dev Cluster`
  * Group: <default>
  * CPU: 2
  * Memory: 2

  Project info:

  * URL: https://cloud.duke.edu/projects/1471

  * Application Console: https://console.apps.dev.okd4.fitz.cloud.duke.edu/k8s/cluster/projects/arctest
  * Cluster Console: https://console.apps.dev.okd4.fitz.cloud.duke.edu//topology/ns/arctest
  * Platform: OKD4 Dev Cluster

* Open application console: https://console.apps.dev.okd4.fitz.cloud.duke.edu/k8s/cluster/projects/arctest

* In the top right corner Click "Add To Project" ( :heavy_plus_sign:) then "Import YAML/JSON" - this will open up a "Import YAML/JSON" dialog

* Copy the contents of `openshift/shiny-server.yaml` and paste it into the "Import YAML/JSON" dialog

* Click "Create"

* Update parameters (Cliclk YAML in Developer>templates>template details)

  * Line 138 - APP_GIT_URI = `https://github.com/johnpfay/HostingShinyApps.git`
  * Line 144 - REPO_DOCKER_FILE = `examples/hello-shiny/Dockerfile` (*same for now)
  * Save

* CLI

  * Download [CLI interface](https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-windows.zip): https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/

  * Open CLI via cmd prompt

    * `oc login --token=sha256~<xxx> --server=https://api.dev.okd4.fitz.cloud.duke.edu:6443`

  * Get log in command from web interface: under username

    * Run in CLI

  * Switch to arctest project: `oc project arctest`

  * Process the template: 

    ```bash
    oc process shiny-server-template | oc apply -f -
    ```

* http://shiny-app-arctest.apps.dev.okd4.fitz.cloud.duke.edu/ Success!

### Task 3: Modify and check

* In app.R, line 27, change max from 1000 to 3000. 
* Commit change
* Build on okd:
  * Developer>Topology>Build #3
