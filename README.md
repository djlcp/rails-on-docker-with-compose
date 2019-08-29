# Running Rails on Docker via Docker Compose

The scripts in the v5 and v6 folders correspond to Rails 5 and Rails 6 respectively. Before starting you will have installed docker (either desktop or toolbox). The instructions and contents are almost identical for both - the real difference being one extra step for Rails 6 and an updated version of Ruby / the Rails gem, plus the addition of Yarn to the container.

I have assumed that if you are running windows you will use Powershell to run commands.

## Installation

1. Create a folder for your rails app to live in
2. Copy all of the files from v5 or v6 in to your folder
3. Update docker-compose.yml and replace `//c/Users/<<YOU>>/<<YOUR FOLDER>>` with the full path to your folder (which should be under the c/Users to avoid problems). I used `//d/Users/mattc/v6test/` for instance
  1. If the path you use is NOT under c/Users, you will need to share it in docker desktop (or virtualbox for docker-toolbox)
4. Update docker-compose.yml to change `<<DESIRED SQL PASSWORD>>` to whatever you wish to use (use something new or random!)
6. If on windows: Using powershell, go into your target folder and do set COMPOSE_CONVERT_WINDOWS_PATHS=1
7. Run: docker-compose build
8. Run: docker-compose run web rails new . --force --no-deps --database=mysql
9. When this has finished, edit the resulting config/database.yml. Change the default section so that 'host' is set to 'db' and the password matches the one you chase above, e.g:

`
    default: &default  
    
      adapter: mysql2  
      encoding: utf8  
      pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>  
      username: root  
      password: <<YOUR CHOSEN PASSWORD>>  
      host: db
`

10. Run: docker-compose up 
11. Wait for mysql to finish initialising itself and interrupt this via Ctrl-C
12. Run: docker-compose web rails db:create
13. IF building Rails 6, run: docker-compose run web rails webpacker:install

Now you're ready to go!

To start your rails stack, run:

docker-compose up

And to stock it run:

docker-compose down

Your new Rails app will be available on http://localhost:3000
