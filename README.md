# vYetti - Animated vSphere Login UI customization

![Alt Text](vYetti-Demo.gif)

## Requirements

* vCenter Server Appliance 6.5+ or 6.7

## Instructions

**Step 1** - Lets first take a backup of the original ROOT.war file so you can easily revert back in case you run into any issues. To do so, login to VCSA via SSH and run the following command:
```
cp /usr/lib/vmware-sso/vmware-sts/webapps/ROOT.war /usr/lib/vmware-sso/vmware-sts/webapps/ROOT.war.bak
```
**Step 2** - We are now going to create a temporary directory that we will use to store the ROOT.war file and we will also copy over it over using the following two commands:
```
mkdir /root/ROOT
cp /usr/lib/vmware-sso/vmware-sts/webapps/ROOT.war /root/ROOT/
```
**Step 3** - We are now going to change into our /root/ROOT directory and extract the contents of the war file so we can make our modifications. To do so, run the following three commands:
```
cd /root/ROOT
unzip ROOT.war
rm ROOT.war
```
**Step 4** - If you wish to pre-fill the credentials when logging into the vSphere Client (Flex/H5), go ahead and update WEB-INF/views/unpentry.jsp as outlined in this blog article [here](https://www.virtuallyghetto.com/2015/08/quick-tip-pre-filled-credentials-in-the-vsphere-6-0-web-client.html). If you wish to make the login button active so you do not have to click into the box or hit tab, you will need to comment out $('#submit').prop('disabled',true); on L91 in **resources/js/websso.js**

**Step 5** - Copy **[app.component.css](app.component.css)** to **/root/ROOT/resources/css/** directory

**Step 6** - Copy **[login-animation.js](login-animation.js)** to **/root/ROOT/resources/js/** directory

**Step 7** - Copy **[unpentry.jsp](unpentry.jsp)** to **/root/ROOT/WEB-INF/views/** directory

**Note 1:**  If you are **NOT** using VCSA 6.7, you should use the original unpentry.jsp and apply the specific animated login code, as the file is different in older versions of VCSA

**Step 8** - Copy **[TweenMax.min.js](https://cdnjs.cloudflare.com/ajax/libs/gsap/1.20.3/TweenMax.min.js)** **/root/ROOT/resources/js/** directory

**Step 9** - Next, we need to re-generate the ROOT.war file so we can replace it with our modified version. To do so, run the following command:
```
zip -r /root/ROOT.war index.jsp META-INF resources WEB-INF
```
If the command was successful, we should have our new ROOT.war located in /root.

**Step 10** - Now we just need to update the system with our modified ROOT.war by simply copying it back to the original location by running the following command:
```
cp /root/ROOT.war /usr/lib/vmware-sso/vmware-sts/webapps/ROOT.war
```
Finally, to verify that that our changes will go into effect, we can simply issue a reboot to our VCSA and we should see that any customization changes made to the vSphere Client Login UI will now persist after a system restart.

**If someone knows an easier way to bring the changes into effect, so please leave a message**
*(especially while testing it's very frustrating to reboot hole VCSA everytime)*
