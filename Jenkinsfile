#!groovy
@Library('roboshop-shared-libraries') _

// responsibility to pass what type of application and component is this pipeline decission

def comfigMap = { 
    application= "nodejsVM",
    component= "catalogue"
}

if( ! env.BRANCH_NAME.equalsIgnoreCase("main")){
    pipelineDecission.decidePipeline(configMap)
}

else{
    echo "This is PRODUCTION, deal it with CR process"
}