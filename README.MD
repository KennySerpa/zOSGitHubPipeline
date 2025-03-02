# zOS Mainframe pipline

### Purpose
This repository has a proof of concept. Where using GitHub actions, as a runner can setup and automate tasks to the mainframe.  

This pipeline will perform a ZOWE command that retrieves all the members from a PDS.

#### Requirements:
- ##### IBM Z Xplore learning account
    - Earn official IBM badges and test this pipeline
        - [Sign up link to IBM](https://ibmzxplore.influitive.com/users/sign_in)
        - IBM has training courses to setup things like VSCODE, Node, Zowe etc. It is highly recommended you take some of their courses to set this environment up.
    - VSCode
        - Extensions within VSCode:
            - ZOWE
            - Github Actions
            - IBM Z Open Editor
            - Use of GitHub Secrets
## File Structure
Your Main script will use the zowe-config folder files as templates to create your Zowe profile within the runner. The runner is a temporary Ubuntu Machine that will process your script commands. Like setup Node / Zowe CLI and more.
```
├── .github            
│   ├── workflows
│   │   └── zowe-test.yml      - Main Script
│   └── zowe-config            - ZOWE configuration template 
│       ├── zowe.config.json
│       └── zowe.schema.json
|
├── .gitignore
├── zowe.config.json  # Note: These next to files are set to ignore. They will not be in this repo.
├── zowe.schema.json
└── README.MD
```

## After cloning this repo

In order to make this script work there are a few control variable's you need to add.  

Setup Secrets  in github along with some variables. (You can do this in the VSCODE IDE)

In this script we reference HOST / USER / PASSWORD. You will need to create this in Github. Below is the code that references this fields:
```
ZOSMF_HOST          #  IP address learning in your ZXP training          
ZOSMF_USER          #  ZUID Mainframe Login when you register IBM ZXP
ZOSMF_PASSWORD      #  Securely stores Mainframe Password
```


```YAML
# zowe-test.yml

            sed "s|\${ZOSMF_HOST}|${{ secrets.ZOSMF_HOST }}|g" | \
            sed "s|\${ZOSMF_USER}|${{ secrets.ZOSMF_USER }}|g" | \
            sed "s|\${ZOSMF_PASSWORD}|${{ secrets.ZOSMF_PASSWORD }}|g" > ~/.zowe/zowe.config.json

```

These are non secrete fields that are not securely stored. They are how you control the command. This command retrieves the Members from your PDS.

```
PDS_HLQ          #  First Qualifier          
PDS_Second_Qual  #  Second Qualifier
```

```YAML
# zowe-test.yml

          zowe zos-files list am '${{ vars.PDS_HLQ }}.${{ vars.PDS_Second_Qual }}'

```