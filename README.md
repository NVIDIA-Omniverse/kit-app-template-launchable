# Kit App Template Launchable

Kit App Template Launchable offers a simplified approach to trying [Kit App Template](https://github.com/NVIDIA-Omniverse/kit-app-template) in a web browser.

Through this project, users can interact with Kit App Template purely from a web browser, with one tab running Visual Studio Code for development and command execution, and another tab providing the streamed user interface for Omniverse. 

Launchables are provided by [NVIDIA Brev](https://developer.nvidia.com/brev), using this repo as a template. Launchables are preconfigured, fully optimized compute and software environments. They allow users to start projects without extensive setup or configuration.

## What this project contains
Kit App Template is cloned via Docker, such that it can be used locally, or deployed on services such as NVIDIA Brev and run with cloud resources.

This project includes:
- Visual Studio Code container
- Kit App Template pre-installed
- Omniverse Kit App Streaming client, based on the [web-viewer-sample](https://github.com/NVIDIA-Omniverse/web-viewer-sample) project.

## Quickstart Guide
This guide will get you started with a Visual Studio Code instance with Kit App Template preinstalled, and an in-browser user interface provided by Kit App Streaming.

> [!NOTE]
> Please note that Brev instances are pay-by-the-hour. To make the best use of credits, stop instances when they are not in use. Stopped instances have a smaller storage charge.

### Deploy
1. Click this Deploy Now button -> [![ Click here to deploy.](https://brev-assets.s3.us-west-1.amazonaws.com/nv-lb-dark.svg)](https://brev.nvidia.com/launchable/deploy/now?launchableID=env-34fCURommjwcJI3NUiMzlTTyJKS)
2. In Brev, click the Deploy Launchable button to spin up the instance.
3. Wait for the instance to be fully ready on Brev: running, built, and the setup script has completed (the first launch can take a while)
4. On the Brev instance page, scroll to the "Using Secure Links" section.
5. Click the arrow icon next to the Shareable URL.
6. Login with your NVIDIA Brev account.
7. Inside Visual Studio Code, continue with the [README.md](https://github.com/NVIDIA-Omniverse/kit-app-template-launchable/blob/main/vscode/README.md) instructions. A summary is provided below.
8. Now you're in the Visual Studio Code dev environment! 

## Detailed Guides
### Running Kit App Template - Detailed Guide
The instructions for Kit App Template can be found [here](https://github.com/NVIDIA-Omniverse/kit-app-template). The detailed guide below will be specific for this repository. 
1. In a VSCode terminal, navigate to the directory where Kit App Template has been cloned:
```
cd kit-app-template
```
2. Run the following command to initiate the configuration wizard:
```
./repo.sh template new
```
3. Follow the prompt instructions:
> NOTE: If this is your first time running the template new tool, you'll be prompted to accept the Omniverse Licensing Terms.
* Select what you want to create with arrow keys: **Application**
* Select desired template with arrow keys: **USD Composer**
* Enter name of application .kit file: [set application name]
* Enter application_display_name: [set application display name]
* Enter version: [set application version]
* Enter name of extension: [set extension name]
* Enter extension_display_name: [set extension display name]
* Enter version: [set extension version]
* Do you want to add application layers?: **Yes**
* Use SPACE to toggle **omni_default_streaming: Omniverse Kit App Streaming (Default)**
* Use ENTER to confirm your selection

4. Build your new application with the following command:
```
./repo.sh build
```

5. Initiate your newly created application using:
```
./repo.sh launch -- --no-window
```
6. Select which App you would like to launch: **ApplicationLayerTemplate** [application_name]_streaming.kit
> Make sure that you select the application with the streaming application layer!

7. Open a new browser tab to view the UI.
8. In this tab, paste the same address as the Visual Studio Code server, changing the end of the URL to: `/viewer/`
> Example: if VSCode is at `https://kat.brevlab-1234`, then the UI can be accessed at `https://kat.brevlab-1234/viewer/`
9. After a few seconds you should see the UI in the viewer tab. The first launch may take much longer as shaders are cached.
10. On subsequent relaunches, simply refresh this tab to see the UI.

> [!Important]
> This setup is only intended to be used with one viewer instance. Please only keep one viewer tab open at a time for best results.

## Creating Your Own Launchable
If you'd like a Launchable with more compute, or other custom features, you can fork this repo and / or use this repo but configure a custom Launchable for your projects.

#### Configuring a Custom Brev Launchable

These instructions describe how to create a customized Launchable, similar to the one linked at the beginning of this guide.

1. Log in to the [Brev](https://login.brev.nvidia.com/signin) website.
2. Go to the Launchables category.
3. Click the **Create Launchable** button.
4. Choose the "I have code files in a git repository" option.
```
https://github.com/NVIDIA-Omniverse/kit-app-template-launchable
```
5. Choose **VM Mode - Basic VM with Python installed**, then click Next.
6. On the next page, add a setup script. Under the *Paste Script* tab, add this code:
```bash
#!/bin/bash
# optional: if not using secure links, uncomment the following line and replace with your desired password
# export VSCODE_PASSWORD=your_password
cd /home/ubuntu/kit-app-template-launchable
docker compose up -d
```
7. Optional step: You can add a password to the VSCode Container if you like. For our default Launchable, we use the Secure Links feature, which leverages your NVIDIA account for login (see step 10)

The VSCode container looks for a password to be set via the $VSCODE_PASSWORD environment variable. Add the following environment variable to the setup script. Replace `your_password` with your desired password, or generate it securely.
```bash
export VSCODE_PASSWORD=your_password
```

8. Click Next.
9. Under "Do you want a Jupyter Notebook experience" select "No, I don't want Jupyter".
10. Select the Secure Link tab, and add a secure link named "kat" at port 80.
11. Select the TCP/UDP ports tab.
12. Add rules to open the following ports for streaming:
```
1024
47998
49100
```
13. Click Next.
14. Choose your desired compute.

> [!NOTE]
> GPUs with RT cores are required for Kit App Streaming. 

> [!IMPORTANT]
> The project is not currently compatible with Crusoe instances. AWS has been tested and is used for the example launchable.
15. Choose disk storage, then click Next.
16. Enter a name, then select **Create Launchable**

> [!Note]
> To use a specific [branch of kit-app-template](https://github.com/NVIDIA-Omniverse/kit-app-template/branches), specify the branch name for the environment variable `KIT_APP_TEMPLATE_BRANCH` before building:
```
KIT_APP_TEMPLATE_BRANCH=107.3 docker compose build
```

Congratulations! You now have a custom launchable.


## Troubleshooting
If you run into issues or can't make the web viewer connect, the first thing to check is that all containers are running.
If using Brev, view your GPU Instance page and find the command to open a terminal on your instance.
Once you have a terminal to the instance running the containers, run `docker ps` and note if the following containers are running:
- web-viewer
- vscode
- dev-nginx-1

To restart the containers:
1. From the terminal connected to your Brev instance, run `docker compose down`
2. Now run `docker compose up -d`
3. Confirm containers mentioned above are all running using `docker ps`

## Licensing Terms
By clicking the "Deploy Launchable" button, you agree to the NVIDIA Software License Agreement found here https://github.com/NVIDIA-Omniverse/kit-app-template/blob/main/LICENSE