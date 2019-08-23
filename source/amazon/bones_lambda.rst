==============================================
Bones Lambda (API Gateway and Coral Clients)
==============================================

Follow this `guide <https://builderhub.corp.amazon.com/docs/bones/user-guide/manual-lambda.html>`_ to set up API Gateway and corresponding Java clients. Internally, some lambda functions will be generated for managing a DynamoDB table.

Following is the detailed procedure and bugs I met & fixed.

-----------------------------------------------------------
Bones Environment
-----------------------------------------------------------

Bindle: https://bindles.amazon.com/software_app/B2BIdentity
Posix: b2b-identity-dev

export BINDLE_ID=amzn1.bindle.resource.q62encgmejylnchp5aaq
export BINDLE_NAME=B2BIdentity
export MAIL_LIST=b2b-identity-dev@amazon.com
export POSIX_TEAM=ab-identity-dev 
export PREFIX=AddLEGReq   
export ORGANIZATION=retail
export ACCT_DATA_ARG="--account_id=863944306515"

-----------------------------------------------------------
Generate Packages
-----------------------------------------------------------

export NAME="AddLegalEntityRequest"
brazil ws create -name ${PREFIX}
cd ${PREFIX}
export LAMBDA_RUNTIME=java_coral 

/apollo/env/OctaneBrazilTools/bin/brazil-octane pkg generate BONES \
    --bindle_guid "${BINDLE_ID}" --org_prefix "${PREFIX}" \
    --organization "${ORGANIZATION}" --mail_list "${MAIL_LIST}" \
    --name "${NAME}" --posix_team "${POSIX_TEAM}" ${ACCT_DATA_ARG}  \
    --lambda_runtime_type "${LAMBDA_RUNTIME}" --create_lambda_pipeline --metrics


.. topic:: Error
  
    Some git operations failed Command '/apollo/env/SDETools/bin/git commit -a -m commit\ by\ Octane' failed!

-----------------------------------------------------------
Generate Pipeline
-----------------------------------------------------------

cd src
/apollo/env/OctaneBrazilTools/bin/brazil-octane ws promote \
  --mailingList "${MAIL_LIST}" --push --bindleName "${BINDLE_NAME}"
brazil ws use -vs BONESLPT/release
cd "${NAME}LambdaLpt"
./configuration/bin/create_pipelines.sh

.. topic:: Error

    Failed to promote the following packages: AddLegalEntityRequestLambdaModel
    [2019-06-17 21:07:22.586477]: Please use 'brazil-octane package promote' individually on these packages to promote them if they are workspace local packages

    Dependent service error: 'Coul not create the Software Package Bindles resource: 2 policies exist for actor amzn1.bindle.actor-type.tedam amzn1.abacus.team.lo56kanu22wz2y633ria and resourceId amzn1.bindle.resource.gekedzo3webkak5iyy3q. Unable to choose one to update. If you receive this error, please cut us a ticket to resolve this issue.'. Please cut an issue to Octane at http://tiny.amazon.com/swjji9ay

    **Solution** Manually promote the package:

     cd AddLegalEntityRequestLambdaModel 
    /apollo/env/OctaneBrazilTools/bin/brazil-octane package promote \
      --mailingList "${MAIL_LIST}" --push --bindleName "${BINDLE_NAME}"


cd "../${PREFIX}BaseLpt"
brazil-build
cd "../${NAME}LambdaLpt"

brazil-build synthesize-cloud-formation
brazil-build synthesize-hydra

.. topic:: Error

    FAILED EXECUTING 'rake' FOR Ruby23x-1.0! (411.234669382s)
    FAILED BUILDING FOR Ruby23x-1.0! (411.235836803s)
    FAILED BUILDING PACKAGE! (411.235933231s)
    Executing 'rake' under Ruby23x-1.0 exited with status 1!
                                                            
                        BUILD FAILED 

    **Solution** Build succeeded after retrying synthesizing pipeline
    Note: This error also occurs when runing rde on Mac


brazil-build synthesize-pipeline

-----------------------------------------------------------
Clean up
-----------------------------------------------------------

cd ../${PREFIX}BaseLpt
git add .
git commit -m "Adds pipeline IDs"
git push --set-upstream origin mainline
brazil vs addtargets -vs "${NAME}Lambda/development" \
  -mv "${NAME}Lambda-1.0"

.. topic:: Error

    "brazil versionset addtargets" failed because of an error (Com::Amazon::Devtools::Bmds::Generated::InvalidRequestException) in a downstream service.
    Please try again; if the error persists, follow the troubleshooting guide:

    http://w?BrazilCLI_2.0/Troubleshooting

    Exception details: The package [AddLegalEntityRequestLambda-1.0] does not exist in the version set [AddLegalEntityRequestLambda/development]. (Com::Amazon::Devtools::Bmds::Generated::InvalidRequestException)
    from:   $ENVROOT/ruby2.1.x/lib/ruby/site_ruby/2.1.0/coral/call.rb:52:in `call'


    **Solution** [This may because previously,*Model was not promoted correctly]

    At https://build.amazon.com/, build AddLegalEntityRequestLambdaModel
    Then build AddLegalEntityRequestLambda


-----------------------------------------------------------
Begin Developing
-----------------------------------------------------------

cd ../${NAME}Lambda
brazil ws use -vs ${NAME}Lambda/development
brazil-build

.. topic:: Error

    2019-06-18 16:22:34 INFO [AddLegalEntityRequestLambdaModel-1.0/runtime] Rebuilding environment because it doesn't exist at /home/niyixuan/workplace/AddLEGReq/env/AddLegalEntityRequestLambdaModel-1.0/runtime
    WARNING: Duplicate key "Jackson-core" in scope [package.AddLegalEntityRequestLambdaTests, resolves-conflict-dependencies, 1.0]
    WARNING: Duplicate key "Jackson-core" in scope [package.AddLegalEntityRequestLambdaTests, resolves-conflict-dependencies, 1.0]
    2019-06-18 16:22:35 INFO [AddLegalEntityRequestLambdaModel-1.0/runtime] Resolving dependencies
    2019-06-18 16:22:35 INFO [AddLegalEntityRequestLambdaModel-1.0/runtime] Building symlink farm of 1 packages from package cache
    2019-06-18 16:22:35 INFO [AddLegalEntityRequestLambdaModel-1.0/runtime] Caching runtime components of dependencies
    2019-06-18 16:22:35 SEVERE Couldn't find a build directory at /home/niyixuan/workplace/AddLEGReq/build/AddLegalEntityRequestLambdaModel/AddLegalEntityRequestLambdaModel-1.0/RHEL5_64/DEV.STD.PTHREAD/build; perhaps you need to build it in your workspace?
    Failed to bootstrap /home/niyixuan/workplace/AddLEGReq/env/AddLegalEntityRequestLambdaModel-1.0/runtime


    **Solution**

    dev-dsk-niyixuan-1d-8344f87b % brazil-recursive-cmd --allPackages brazil-build build

    Or bb under AddLegalEntityRequestLambdaModel first, then bb normally.



-----------------------------------------------------------
Pull from remote
-----------------------------------------------------------

export NAME=AddLegalEntityRequest
ws create --root ~/workplace/${NAME} --versionset ${NAME}Lambda/development
brazil ws use --package ${NAME}Lambda --package ${NAME}LambdaClientConfig --package ${NAME}LambdaJavaClient --package ${NAME}LambdaLpt --package ${NAME}LambdaModel --package ${NAME}LambdaTests

rde wflow run

.. topic:: Error

    [14:41:44-E] 1 job(s) execution failed
    Details:
        the build job failed
        failed to fetch the package list for workspace "/Users/niyixuan/workplace/AddLegalEntityRequest"
        failed to load a new list of topologically ordered packages for workspace "/Users/niyixuan/workplace/AddLegalEntityRequest"
        failed to get checked out dependencies for package "AddLegalEntityRequestLambdaTests" in workspace "/Users/niyixuan/workplace/AddLegalEntityRequest"
        exit status 1

    **Solution**

    [Failed because some dependencies are not added to the version set.]
    Run `brazil-recursive-cmd --allPackages brazil-build build`
    Update the version sets [can do it beforehand at https://build.amazon.com/merge]
    Then `brazil ws --sync --md`
    [Check config to add all dependencies at once]


-----------------------------------------------------------
Invoke local endpoint
-----------------------------------------------------------

curl http://localhost:1180/requests
curl http://localhost:1180/requests\?businessId=1 -X POST
curl http://localhost:1180/requests\?requestId=\&businessId= -X DELETE 
curl http://localhost:1180/requests\?limit=1  
curl http://localhost:1180/requests\?limit=1\&nextToken=

curl http://localhost:1180/view-requests
curl http://localhost:1180/view-requests\?limit=1
curl http://localhost:1180/view-requests\?limit=1\&nextToken=
curl http://localhost:1180/view-request\?businessId=1\&requestId=
curl http://localhost:1180/manage-request\?businessId=1 -X POST
curl http://localhost:1180/manage-request\?businessId=1\&requestId= \&name=test -X PUT
curl http://localhost:1180/manage-request\?businessId=1\&requestId= -X DELETE


scp -r niyixuan.aka.corp.amazon.com:/workplace/niyixuan/AddLegalEntityRequest/build/AddLegalEntityRequestLambda/AddLegalEntityRequestLambda-1.0/RHEL5_64/DEV.STD.PTHREAD/build/brazil-unit-tests ./


-----------------------------------------------------------
Pipeline Approval Workflow Error
-----------------------------------------------------------

Pipeline Approval workflow failed at beta stage

Hydra Integ Tests in us-west-2 beta Failed  [Consider running 'bb synthesize-hydra' in your service LPT package.] BATS transform info: exception: bats_workflow.exceptions.BootstrapFailed cause: "Failed to bootstrap package AddLegalEntityRequestLambdaTests-1.0" in file "/apollo/env/BATSWorkflow2/lib/python3.6/site-packages/bats_workflow/utils.py", line 128, in run_process: raise exception(message) from e transform_type: AWSLambda-1.0 transform_id: df09c239-eb34-410b-908d-b253873a9561 versionset: AddLegalEntityRequestLambda/development event_id: 5820692578 name: AddLegalEntityRequestLambdaTests major_version: 1.0 closure: test-runtime platform: RHEL5_64 Test package build status is failed . 

.. topic:: Solution

    cd ../AddLegalEntityRequestLambdaLpt bb synthesize-hydra
    Merge everything
    brazil ws --sync --md

    [Need to build baseLpt first]

-----------------------------------------------------------
Hydra Test
-----------------------------------------------------------

rde wflow run
rde wflow run -s run-hydra-tests


.. topic:: Error

    Approval Workflow failed

    That change set could not be found - or you don't have permission to view it.
    Something unexpected happened 😞. Message: User is not allowed to see details for this event.

    **This is not solved yet**. Modified the approval procedure to accept all the changes.

-----------------------------------------------------------
Debugging
-----------------------------------------------------------

View log:

rde stack exec AddLegalEntityRequestLambda
cd rde/logs/
less api-gateway.log


-----------------------------------------------------------
AWS
-----------------------------------------------------------

AWS credentials:
  
  ``~/.aws/credentials``