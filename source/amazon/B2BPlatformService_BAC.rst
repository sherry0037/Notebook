====================================================
B2BPlatformService and BusinessAccountConsole 
====================================================

----------------------------------
B2BPlatformService
----------------------------------
1. Install IntelliJ
    1. Install toolbox https://w.amazon.com/index.php/BuilderToolbox/GettingStarted#Install_Toolbox
    2. Install BrazilCLI 2.0
    3. Install crux
2. Set up the Black Caiman IntelliJ plugin
3. Set up NinjaDevSync between your laptop and cloud developer desktop.
4. Set up B2BDevelopmentService https://w.amazon.com/index.php/BISS/Development/Projects/B2BPlatformService/DevelopmentSetup
    1. Create a local workspace for the B2BPlatformService brazil package using version set B2BPlatformService/development 
        cd /workplace/niyixuan/B2BPlatformService
        brazil ws --create --name niyixuan-B2BPlatformService --root /workplace/niyixuan/B2BPlatformService --vs B2BPlatformService/development
    2. Pull packages:
        1. brazil ws use -p BusinessAccountConsole --branch mainline
    3. To build all packages in your workspace at once, navigate to the top level package (B2BPlatformService in this case) and run: 
            brazil-recursive-cmd-parallel "brazil-build" 
            brazil-recursive-cmd -all brazil-build
    2. Child environment: https://apollo.amazon.com/environments/B2BPlatformServiceAAAShim/niyixuan/
    3. Pipeline: https://pipelines.amazon.com/pipelines/B2BPlatformServiceAAAShim
5. Configure RDE using the instructions found on the wiki
6. Set up BusinessAccountConsole
    1. Create 1-click child environment: https://apollo.amazon.com/environments/BusinessAccountConsole/US/stages/Prod
    2. Child environment: https://apollo.amazon.com/environments/BusinessAccountConsole/US/niyixuan/
    3. Set the Apache OP Config httpServerName to your CNAME (e.g. weberd.aka.corp.amazon.com)
        1. httpExternalRegularPort 3080
        2. httpExternalSecurePort 3043
    4. Create a Beta stage
        1. Hosts and Hostclasses: DEV-DSK-NIYIXUAN
        2. Network Location: PDX/corp
    5. Deployment: sync from Beta
    6. release build
        1. cd src/BusinessAccountConcole
        2. brazil-build release

7. Run rde wflow run to start everything up
8. Visit your RDE console at http://niyixuan.aka.corp.amazon.com:5143/


KeyMasterDaemon/Prod version 532825159 will be included with this deployment. Undo
AAASecurityDaemon/Prod version 501127889 will be included with this deployment. Undo


https://niyixuan.aka.corp.amazon.com:5143/

rde stack stop BusinessAccountConsole

----------------------------------
Error
----------------------------------

[22:01:12-E] failed to start personal stack
Details:
    failed to start development stack
    1 job(s) execution failed
    failed to setup network mapping for app "BusinessAccountConsole"
    error occurred while setting up endpoints for app BusinessAccountConsole
    failed to setup and start container for endpoint "debug"
    failed to start container for endpoint "debug"
    API error (500)
    {"message":"driver failed programming external connectivity on endpoint debug (581b0fd7d3e825d3301ceae2e1b1b2efe07405c3c605f28b05cc7424bcc20794)
    Error starting userland proxy
    listen tcp 127.0.0.1:3083
    bind
    address already in use"}

Solution:
netstat -nalp | grep 3083  

(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 127.0.0.1:3083              0.0.0.0:*                   LISTEN      26060/jsvc.exec    

ps -ef | grep 26060

apolloRun -e BusinessAccountConsole -a Deactivate
rde stack stop BusinessAccountConsole

If not working after changing the DNF:
rde stack exec BusinessAccountConsole bash
/apollo/env/BusinessAccountConsole/var/output/logs

less definition.yaml 

Check DNS:
Ping/dig niyixuan.aka.corp.amazon.com


