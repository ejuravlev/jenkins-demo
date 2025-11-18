def httpRequestBuilderMap(String url, token = false){
    return [
        httpMode     : 'GET',
        url          : url,
        customHeaders: [
            [name: "Cache-Control", value: 'no-cache'] +
            (token ? [name: "Authorization", value: "Bearer " + token] : [:])
        ],
        contentType     : 'APPLICATION_JSON',
        acceptType      : 'APPLICATION_JSON',
        ignoreSslErrors : true
    ]
}

def httpDeleteWithPrivateToken(String url, String token) {

    def cfg = httpRequestBuilderMap(url, token)

    def newHeaders = [
        [name: "Cache-Control", value: "no-cache"],
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

pipeline {
    agent any

    stages {
        stage('Test DELETE') {
            steps {
                script {
                    def resp = httpDeleteWithPrivateToken(
                        "https://my-gitlab.com/api/v4/projects/3/merge_requests/10",
                        "123abc"
                    )

                    echo "Status: ${resp.status}"
                    echo "Body: ${resp.content}"
                }
            }
        }
    }
}
