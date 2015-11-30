Immutant on OpenShift
=====================

    export yourapp=scalledas7

    rhc app create -a $yourapp -s -t jbossas-7 --from-code git://github.com/openshift-quickstart/immutant-quickstart.git
    
OR *OR SIMPLY COPY PASTE SOURCES TO YOUR CLONED* app and then Just:
    git commit -am 'Your real app now placed here'
    git push

OR for real add from repo:

    *git remote add $yourrealapp -m master git://github.com/immutant/cluster-demo.git && git pull -s recursive -X theirs $yourrealapp master*

and then Just
    git push
    
THAT'S ALL!!!
THAT'S ALL!!!
THAT'S ALL!!!
    
    #(OPTIONAL, NOT REQUIRED, Use above if it's ok) 
    only use if you have problem with code above. More verbose almost identical install
    
    rhc app create -s $yourapp jbossas-7 

If not cloned then
git clone ssh bla bla bla from application menu on openshift

    cd $yourapp 
    rm -rf pom.xml src && git remote add quickstart -m master git://github.com/openshift-quickstart/immutant-quickstart.git && git pull --no-commit -s recursive -X theirs quickstart master && git add -A . && git commit -m "Add Immutant modules and setup Clojure project" && git push


Here is a quick way to try out your Leiningen-based Clojure
application running in Immutant on OpenShift.

By default, this quickstart will install the latest incremental
version of Immutant. You can specify a different version by tweaking
`.openshift/action_hooks/pre_start_jbossas`.

One particularly nice thing about OpenShift is that it provides simple
ssh port forwarding, so you can configure your app to start either a
Swank or nREPL server and connect to it via an ssh tunnel. The only
stipulation is that the port you specify be between 15000-35530. So in
your `project.clj`, you might add:

    :immutant {:nrepl-port 24005}

You can then run `rhc port-forward -a yourapp` to setup your tunnel.
These and other `:immutant` options can optionally be specified in the
deployment descriptor: `deployments/your-clojure-application.clj`.

Running on OpenShift
--------------------

Create an account at https://www.openshift.com

Ensure you have the latest version of the
[client tools](https://www.openshift.com/get-started#cli).

Create a jbossas-7 application

    rhc app create yourapp jbossas-7

Remove the sample app provided by the jbossas-7 cartridge

    cd yourapp
    rm -rf pom.xml src

Add this upstream repo

    git remote add quickstart -m master git://github.com/openshift-quickstart/immutant-quickstart.git
    git pull --no-commit -s recursive -X theirs quickstart master

Then add, commit, and push your changes

    git add -A .
    git commit -m "Add Immutant modules and setup Clojure project"
    git push

That's it! The first build will take a minute or two, so be patient.
You should ssh to your app and run `tail_all` so you'll have something
to watch while your app deploys.

When you see `Deployed "your-clojure-application.clj"` in the log,
point a browser at the following link (adjusted for your namespace)
and you should see a friendly welcome:

    http://yourapp-$namespace.rhcloud.com

Any changes you push from the `yourapp/` directory will trigger a
redeploy of your app.

At this point, you can either write your app from scratch, or you can
merge in changes from an existing project, i.e. your real app. Let's
use a small app demonstrating the various Immutant clustering
features, for example:

    git remote add yourrealapp -m master git://github.com/immutant/cluster-demo.git
    git pull -s recursive -X theirs yourrealapp master
    git push

Now whenever you're ready to deploy your real app to OpenShift, you
pull in changes from your real repo and push to your OpenShift repo.

By the way, to see the clustering features in that demo app, you'll
need to add a gear, like so:

    rhc scale-cartridge jbossas-7 -a yourapp 2

Have fun, and drop in to the `#immutant` IRC channel on freenode.net
if you have any questions.
