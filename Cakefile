#-----------------------------------------------------------------
# BUILD WITH CAKE
#
#  source: https://github.com/jashkenas/coffee-script/wiki/%5BHowTo%5D-Compiling-and-Setting-Up-Build-Tools

fs     = require 'fs'
{exec} = require 'child_process'


task 'module', 'loads needed node.js modules', ->

    out = (err, stdout, stderr) ->
        throw err if err
        console.log stdout + stderr
        
    exec 'npm install express', out
    exec 'npm install jade', out
    exec 'npm install stylus', out
    exec 'npm install coffee-script', out
    exec 'npm install socket.io', out
    exec 'npm install redis', out
    exec 'npm install hiredis', out
    exec 'npm install async', out





# todo:

appFiles  = [
    # omit src/ and .coffee to make the below lines a little shorter
    'content/scripts/statusbar'
    'content/scripts/command/quickMacro'
    'content/scripts/command/selectionTools/general'
]

task 'build', 'Build single application file from source files', ->
    appContents = new Array remaining = appFiles.length
    for file, index in appFiles then do (file, index) ->
        fs.readFile "src/#{file}.coffee", 'utf8', (err, fileContents) ->
            throw err if err
            appContents[index] = fileContents
            process() if --remaining is 0
    process = ->
        fs.writeFile 'lib/app.coffee', appContents.join('\n\n'), 'utf8', (err) ->
        throw err if err
        exec 'coffee --compile lib/app.coffee', (err, stdout, stderr) ->
            throw err if err
            console.log stdout + stderr
        fs.unlink 'lib/app.coffee', (err) ->
            throw err if err
            console.log 'Done.'

task 'minify', 'Minify the resulting application file after build', ->
    exec 'java -jar "/home/stan/public/compiler.jar" --js lib/app.js --js_output_file lib/app.production.js', (err, stdout, stderr) ->
        throw err if err
        console.log stdout + stderr
