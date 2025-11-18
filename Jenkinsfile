pipeline {
    agent any

    stages {
        stage('Delete') {
            steps {
                script {

                   def httpRequestBuilderMap(String url, token = false){
                        return [
                            httpMode     : 'GET',
                            url          : url,
                            customHeaders: [[name: "Cache-Control", value: 'no-cache'] + (token ? [name: "Authorization", value: "Bearer " + token] : [:])],
                            contentType: 'APPLICATION_JSON',
                            acceptType: 'APPLICATION_JSON',
                            ignoreSslErrors: true
                        ]
                    }

                    def httpDeleteWithPrivateToken = { url, token ->
                        def req = httpRequestBuilderMap(url, token)
                        req.httpMode = 'DELETE'

                        req.customHeaders = [
                            [name: "Cache-Control", value: 'no-cache'],
                            [name: "PRIVATE-TOKEN", value: token]
                        ]

                        return httpRequest(
                            httpMode: "DELETE",
                            url: cfg.url,
                            customHeaders: newHeaders,
                            contentType: cfg.contentType,
                            acceptType: cfg.acceptType,
                            ignoreSslErrors: cfg.ignoreSslErrors
                        )
                    }

                    def response = httpDeleteWithPrivateToken(
                        "https://gitlab.example.com/api/v4/projects/123",
                        "myPrivateToken"
                    )

                    echo "Status: ${response.status}"
                    echo "Body: ${response.content}"
                }
            }
        }
    }
}