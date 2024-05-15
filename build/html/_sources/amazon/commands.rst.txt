==========================
Useful Commands
==========================

----------------------------------
Code Review
----------------------------------

- Update code review:
    ``cr --update-review CR-``

- Include multiple packages:
    ``cr -i [pk1] -i [pk2]``
    ``cr --include [pk1], [pk2]``

- Include only the latest commit
    ``cr -i [pk1][HEAD^]``

- Dry run build:
    ``brazil workspace --dryrun``


----------------------------------
Debugging
----------------------------------

- Show lines includ "ERROR"
    cat BusinessAccountConsole.2019-07-12-20 | grep "ERROR" | wc -l
    cat BusinessAccountConsole.2019-07-12-20 | grep "ERROR" | less 

- Go inside RDE and check logs
    Rde stack exec BusinessAccountConsole
    cd /apollo/env/BusinessAccountConsole/var/output/logs/
    find . | grep log


----------------------------------
Cloud Desktop
----------------------------------

- Your Cloud Desktop host type
    *  M4-2xlarge

- The correct fleet
    * Dev-dsk

- Hose name
    dev-dsk-niyixuan-1d-8344f87b.us-east-1.amazon.com

- Group
    ab-identity-dev

- CNAME (Alias) 
    niyixuan-clouddesk.aka.corp.amazon.com
    niyixuan.aka.corp.amazon.com

- Test after build:
    http://dev-dsk-niyixuan-1d-8344f87b.us-east-1.amazon.com:2222/explorer/index.html


----------------------------------
DynamoDB Local
----------------------------------

https://w.amazon.com/index.php/RDE/Marketplace/AWS_Resources

brazil ws create -n rde -vs live
cd rde
brazil ws use -p RDE-DynamoDBLocal
cd src/RDE-DynamoDBLocal
rde wflow run provision
rde wflow run
rde stack show -a

When rde wflow run under application package,
Do `rde stack provision`
Need to add version set [https://builderhub.corp.amazon.com/blog/easier-rde-stack-provisioning-through-dedicated-step-and-command/#howcaniusethisnewfunctionality]
 
DynamoDBLocal:
  …
  apolloShimURI: ../../../rde/src/RDE-DynamoDBLocal/apollo-shim/DynamoDBLocal.json
  versionSet: DynamoDBLocal/transactions
  …


