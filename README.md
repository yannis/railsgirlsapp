# Deploy to Hidora

## Deploy via direct upload

Before doing anything on Hidora, you have to add a file to your app. Indeed, the Jelastic stack on which Hidora is based can perform post deployment application configuration via `rake`. This is usually needed to finalize configuration of complex applications, to run additional applications or specific steps for application configuration like `db:migrate`. We will use this mechanism to create and migrate our database upon deploy. [more info](https://docs.jelastic.com/ruby-post-deploy-configuration)

For this, we will create a `rake_deploy` file at the root of our application with the following content:

```
db:create RAILS_ENV=production
db:migrate RAILS_ENV=production
assets:precompile RAILS_ENV=production
```

Once done, log in Hidora and create a new environment

![](/readme_images/1.png)

Chose "Create a new environment" and click on "Ruby" at the top of the modal window.

![](/readme_images/2.png)

### SSL setup

Start by setting 'SSL' by chosing the default Jelastic SSL.

SSL (Secure Sockets Layer) is the standard security technology for establishing an encrypted link between a web server and a browser. This link ensures that all data passed between the web server and browsers remain private and integral. It's now somehow required by default by most users and Chrome now mark website that don't use it as "not secure".

![](/readme_images/3.png)

### Balancing setup

Load balancing across multiple application instances is a commonly used technique for optimizing resource utilization, maximizing throughput, reducing latency, and ensuring fault-tolerant configurations. Even if you don't use multiple application instances yet, it's good to set it up now to be future-proof.

![](/readme_images/4.png)

You can give to your environment the name you want.

![](/readme_images/5.png)

### Ruby server setup

We also need a server that support Ruby. Hidora is proposing [Phusion Passenger](https://en.wikipedia.org/wiki/Phusion_Passenger) as the default Ruby server which is a good choice. Here, we will just increase the ruby version to the last one, 2.4.1.

You can give to your environment the name you want.

![](/readme_images/6.png)

Click the 'Create' button on the bottom right of the modal window to create your environment. This can take several minute.

### Download your project from Codeanywhere

On Codeanywhere, download your project by right-clicking your container and chosing 'Download' from the context menu.

![](/readme_images/7.png)

### Upload your project to Hidora

Back to Hidora, click 'Deployment manager' on the bottom of the window, and the 'Upload' on top of the window that appear.

![](/readme_images/8.png)

Click 'Browse', select the `.zip` file you just downloaded, and click 'Upload' to upload it to Hidora.

![](/readme_images/9.png)

Once uploaded, deploy your app to your environment by clicking the 'Deploy to…' icon beside your file.

![](/readme_images/10.png)

Keep 'production' as deployment type,and click the 'Deploy' button. This will take some time, too.

![](/readme_images/11.png)

Once done, you should have something like this:

![](/readme_images/12.png)

You can click the 'Open in browser' icon to visit your website… and you'll get an error.

![](/readme_images/13.png)

<!-- This is due, to the fact that your database is nor createt or migrated. Moreover your assets are not deployed. To do all this you'll need to `ssh` in your Hidora container.

Click the 'Settings' account on the top level of your container.

![](/readme_images/14.png)

Click SSH Access on the bottom-left menu and follow the instructions. Add a public key if required.

From your terminal, type

`ssh {your node id}@gate.hidora.com -p {your port number}`

Then type the number of your 'UNGROUPED ENV' (for me it's 4).

![](/readme_images/15.png)

Then the number of your 'Nginx Ruby' server (for me it's 3)

![](/readme_images/16.png)

Once in, type `cd /var/www/webroot/ROOT` to access the root of your rails app, then create the database with `rails db:create RAILS_ENV=production` and migrate it with `rails db:migrate RAILS_ENV=production`. Finally compile your assets with `rails assets:precompile RAILS_ENV=production`. -->

Just restart your server by clicking the 'Restart node' icon on the Hidora interface.

![](/readme_images/17.png)

Hurrah, your app should be online!

![](/readme_images/18.png)

Beware that with a SQlite database, your data won't per persisted between deploys. You must switch to another database (PostgreSQl is a good choice) if you want to persist the data.
