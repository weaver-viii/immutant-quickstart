Immutant on OpenShift
=====================

Here is a quick way to try out your Leiningen-based Clojure
application running in Immutant on OpenShift.

By default, this quickstart will install the latest incremental
version of Immutant. You can specify a different version by tweaking
`.openshift/action_hooks/pre_start_jbossas-7`, but any build older 
than incremental 607 won't work. ;-)

One particularly nice thing about OpenShift is that it provides simple
ssh port forwarding, so you can configure your app to start either a
Swank or nREPL server and connect to it via an ssh tunnel. The only
stipulation is that the port you specify be between 15000-35530. So in
your `project.clj`, you might add:

    :immutant {:nrepl-port 24005}

You can then run `rhc port-forward -a yourapp` to setup your tunnel.
These and other `:immutant` options can be specified in the deployment
descriptor: `deployments/your-clojure-application.clj`. You'll notice
we activate the `:openshift` profile in there, which may result in
some harmless warnings in the log unless you a) add that profile to
your project.clj or 2) remove it from the deployment descriptor.

Running on OpenShift
--------------------

Create an account at http://openshift.redhat.com/

Ensure you have the latest version of the
[client tools](https://www.openshift.com/get-started#cli).

Create a jbossas-7 application from the code in this repository:

    rhc app create -a yourapp -t jbossas-7 --from-code git://github.com/openshift-quickstart/immutant-quickstart.git

That's it! The first build will take a minute or two, so be patient.
You should ssh to your app and run `tail_all` so you'll have something
to watch while your app deploys.

When you see `Deployed "your-clojure-application.clj"` in the log,
point a browser at the following link (adjusted for your namespace)
and you should see a friendly welcome:

    http://yourapp-$namespace.rhcloud.com

Any changes you push from the `yourapp/` directory will trigger a
redeploy of your app.

Drop in to the `#immutant` IRC channel on freenode.net if you have any
questions.
