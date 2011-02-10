
## API

 The Engine API at its core is extremely simple, all we need to do is pass
 our http `server` to `engine()`, then call `listen()` as we would on the `http.Server` itself.


     var engine = require('../')
       , http = require('http');

     var server = http.createServer(function(req, res){
       res.writeHead(200);
       res.end('Hello World');
     });

     engine(server)
       .listen(3000);

### Settings

 Below are the settings available:
 
   - `workers`  Number of workers to spawn, defaults to the number of CPUs
   - 'working directory`  Working directory defaulting to `/`
   - 'backlog`  Connection backlog, defaulting to 128
   - 'socket path`  Master socket path defaulting to `./master.sock`

 We can take what we have now, and go on to apply settings using the `set(option, value)` method. For example:
 
    engine(server)
      .set('working directory', '/')
      .set('workers', 5)
      .listen(3000);

### Signals

 Engine performs the following actions when handling signals:
 
   - `SIGINT`   hard shutdown
   - `SIGTERM`  hard shutdown
   - `SIGQUIT`  graceful shutdown
   - `SIGUSR2`  restart workers
   - `SIGHUP`   ignored

### Events

 The following events are emitted, useful for plugins or general purpose logging etc.
 
   - `start`. When the server is starting (pre-spawn)
   - `worker`. When a worker is spawned, passing the `worker`
   - `listening`. When the server is listening for connections (post-spawn)
   - `broadcast`. When master is broadcasting a `cmd` with `args`
   - `closing`. When master is gracefully shutting down
   - `close`. When master has completed shutting down
   - `worker killed`. When a worker has died
   - `kill`. When a `signal` is being sent to all workers
   - `restart`. Restart requested by REPL or signal