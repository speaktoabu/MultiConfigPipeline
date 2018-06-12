def releaseNode = "${RELEASE}".split(',')

git clone https://github.com/speaktoabu/Percona_test.git

stage("Before") {
    node {
		sh "sh start.sh"
    }
}

for(int i=0; i<releaseNode.size();i++){
    def releaseValue = releaseNode[i]
    stage ("$releaseValue") {
        parallel createStages("$releaseValue")
    }
}


stage("after") {
    node {
		sh "sh end.sh"
    }
}

def createStages(releaseValue) {

    def osNode = "${OS}".split(',')
    def dbNode = "${DB}".split(',')
    def tasks = [:]
    for(int i=0; i< osNode.size(); i++) {
        def axisNodeValue = osNode[i]
        for(int j=0; j< dbNode.size(); j++) {
            def axisToolValue = dbNode[j]
            tasks["${axisNodeValue}/${axisToolValue}"] = {
                try{
                   echo "Do your task here for config ${releaseValue}/${axisNodeValue}/${axisToolValue}"
                } catch (err) {
                    echo "Failed"
                }
            }
        }
    }
    return tasks
}
