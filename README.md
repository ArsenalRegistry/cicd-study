
```
            stage('Build Docker image') {
                container('build-tools') {
                    withCredentials([usernamePassword(credentialsId: 'cluster-registry-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 
                        sh "podman login -u ${USERNAME} -p ${PASSWORD} ${dockerRegistry}  --tls-verify=false"
                        sh "podman build -t ${imageName}:${tagName} --build-arg sourceFile=`find target -name '*.jar' | head -n 1` -f devops/jenkins/Dockerfile . --tls-verify=false"
                        sh "podman push ${imageName}:${tagName} --tls-verify=false"

                        sh "podman tag ${imageName}:${tagName} ${imageName}:latest"
                        sh "podman push ${imageName}:latest --tls-verify=false"
                    }
                }
            }
```


```
helm3 lint --namespace appdu-demo12 .
helm3 upgrade --install --namespace appdu-demo12 ${releaseName} ${additionalFlag} .
```
