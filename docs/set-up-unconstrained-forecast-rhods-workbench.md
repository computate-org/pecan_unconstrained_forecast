# Set up unconstrained forecast RHODS Workbench

- Log in to the [NERC OpenShift Console here](https://console.apps.shift.nerc.mghpcc.org). 
- Click on your username in the top right, and select [ Copy login command ]. 
- Paste the command into your terminal. 

## Download the RHODS Unconstrained Forecast Secret

- Download the RHODS Unconstrained Forecast Secret

```bash
oc -n software-application-innovation-lab-sail-projects-fcd6dfa get secret/rhods-unconstrained-forecast -o yaml > ~/Downloads/rhods-unconstrained-forecast-secret.yaml
```

## Set up a RHODS Workbench for the Unconstrained Forecast

- Visit the [RHODS dashboard in NERC](https://rhods-dashboard-redhat-ods-applications.apps.shift.nerc.mghpcc.org/projects/software-application-innovation-lab-sail-projects-fcd6dfa)
- Click the [ Create Workbench ] button. 
- Give your workbench a unique name like `ctate-rhods-pecan`. 
- Image selection: PEcAn Unconstrained Forecast
- Container size: Medium or greater
- In "Environment variables", click "Add variable"
  - Select environment variable type: Secret
  - Select one: Upload
  - Click [ Upload ] and select the `~/Downloads/rhods-unconstrained-forecast-secret.yaml`
- Select "Create new persistent storage"
  - Name: Use the name of your workbench
  - Persistent storage size: 20Gi or greater
- Click [ Create workbench ]

## Create a route for your RHODS workbench

By default, our namespace `software-application-innovation-lab-sail-projects-fcd6dfa` is too long for the default Route creation for RHODS to work correctly, 
so we need to create a route to our RHODS workbench manually. 

- Visit the [Routes for our namespace here](https://console.apps.shift.nerc.mghpcc.org/k8s/ns/software-application-innovation-lab-sail-projects-fcd6dfa/route.openshift.io~v1~Route). 
- Click [ Create Route ]. 
- Name: same as your workbench name
- Hostname: Make sure you give your route a unique hostname that ends with `-rhods-pecan.apps.shift.nerc.mghpcc.org`
- Path: /
- Service: Your workbench name, but ending in `-tls`
- Target port: 443
- Select `Secure route`
- TLS termination: Reencrypt
- Insecure traffic: Redirect
- Don't worry about certificates, leave them blank
- Click [ Create ]

Now you should be able to open your RHODS workbench

# Running an ecological forecast in RHODS

- Open a Terminal in your workbench: File -> New -> Terminal
- Clone a current PEcAn Git Repo

```bash
git clone https://github.com/computate-org/pecan.git -b hf_landscape ~/pecan
```

- Make a directory for `forecast_example` and rsync the forecast_example: 

```bash
mkdir ~/forecast_example/
oc rsync ~/Downloads/forecast_example/ ctate-rhods-pecan-0:/opt/app-root/src/forecast_example/
```

- Try out the HARV_metdownload_efi.R script in the Terminal: 

```bash
Rscript pecan/scripts/HARV_metdownload_efi.R
```