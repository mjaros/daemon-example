# Ruby Daemon Example

Example of a daemon process written in ruby with basic start, stop, restart, status controls.
This is for UNIX based systems only.

## Usage

Put all code that you want to be processed by your daemon into the worker class:

    class Worker
      def initialize
        work
      end

      def work
        while $running
          # Do some stuff
          breakable_sleep(300)
        end
      end
    end

In our loop we are using *$running* and *breakable_sleep()*.

Using *$running* makes sure that our loop will securely finish and not be interrupted in the middle of execution when receiving a TERM signal.

Using *breakable_sleep()* instead of sleep() allows the daemon process to be terminated during sleep. This is especially helpfull if you have a very long running sleep.

## Controlling the daemon

    $ ./daemon
    -> Usage: daemon {start|stop|restart|status}

    $ ./daemon start
    -> Starting... done! (PID is 2465)

    $ ./daemon status
    -> daemon is running

    $ ./daemon restart
    -> Stopping... done!
    -> Starting... done! (PID is 2537)

    $ ./daemon stop
    -> Stopping... done!

## PID File & Process name

On start the daemon will by default create a PID file in it's current working directory with the name of the executable.

So if you name your executable "awesome-daemon" it will be named "awesome-daemon.pid".
Additionally the name of your process shown by *ps* or *top* command will be "awesome-daemon".