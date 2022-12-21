# Rigel
This is a cryptocurrency miner for Nvidia GPUs

![rigel screenshot kaspa](https://user-images.githubusercontent.com/119491468/204991320-802a9de8-8a77-4527-b802-846d542eb05e.png)

## Supported algorithms
* ethash (Ethereum PoW, Zilliqa)
* etchash (Ethereum Classic)
* kheavyhash (Kaspa)
* ethash+kheavyhash
* etchash+kheavyhash
* any single or dual algorithm combination + ZIL

## Developer fee
| Algorithm   | Fee  |
| ----------- | -----|
| ethash      | 0.7% |
| etchash     | 0.7% |
| zil         | 0%   |
| kheavyhash  | 0.7% |

## Features
* Available on Linux and Windows
* Terminal user interface
* Failover pool support
* Temperature, clocks, fan monitoring
* Built-in GPU overclocking support with an option to apply different settings for ZIL
* Temperature limits and fan control
* HTTP API

## Usage
```
  -a, --algorithm <ALGORITHM>
          Selects the mining algorithm
          
          Currently supported:
          etchash     (ETC)
          ethash      (ETHW)
          kheavyhash  (Kaspa)
          zil         (Zilliqa)

  -o, --url <URL>
          Sets the pool URL
          
          Format:
          <mining protocol>+<transport protocol>://<pool hostname>:<port number>
          
          Mining protocols:    stratum, ethproxy, ethstratum, zmp (zil only)
          Transport protocols: tcp, ssl
          
          When dual or triple mining the value should be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.
          Failover pools are set by using `-o` multiple times.
          
          Examples:
          -o stratum+ssl://pool.woolypooly.com:3112
          -o zmp+tcp://zil.flexpool.io:9486
              mine Zilliqa to flexpool using ZMP protocol
          -o [1]stratum+tcp://eth.f2pool.com:6688 -o [2]stratum+ssl://pool.woolypooly.com:3112
              mine the primary algorithm to f2pool and the second algorithm to woolypooly

  -u, --username <USERNAME>
          Sets the username for pool authorisation
          
          When dual or triple mining the value should be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

  -p, --password <PASSWORD>
          Sets the password for pool authorisation
          
          When dual or triple mining the value should be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

  -w, --worker <WORKER>
          Sets the worker name
          
          When dual or triple mining the value should be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

  -d, --devices <D1,D2,...>
          Sets GPU devices to use for mining
          
          Must be a comma-separated list of device indices. If not specified,
          all detected devices will be used. Devices are ordered by their
          PCI bus addresses. First GPU has index of 0.
          To display all available devices, use --list-devices.
          
          Example:
          -d 0,2,4,5

      --list-devices
          List available mining devices and exit

      --dual-mode <MODE1,MODE2,...>
          Controls GPU behaviour in dual mining mode
          
          Format: <algo>:<dual ratio>
          `<algo>` must be one of the following:
              a1  - first algorithm
              a2  - second algorithm,
              a12 - both
          
          `<dual ratio>` is optional, and, if present,
          must be `rXX` (`dual ratio` coefficient set to `XX`)
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          
          Examples:
          --dual-mode a1,_,a12:r5,a2
              GPU#0 will mine the primary algorithm
              GPU#1 will dual mine with default settings
              GPU#1 will dual mine, dual ratio set to 5
              GPU#2 will mine the second algorithm

      --temp-limit <LIMIT1,LIMIT2,...>
          Sets GPU thermal limits
          
          Sets the temperatures of GPU core and memory upon reaching which the miner
          stops the overheated GPU and waits for it to cool down before resuming.
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          
          Examples:
          --temp-limit tc[60-70]
              stops the GPU when its core temperature reaches 70 degrees celsius
              and resumes mining when it cools down to 60 degrees
          
          --temp-limit tm[105-115]
              stops the GPU when its memory temperature reaches 115 degrees celsius
              and resumes mining when it cools down to 60 degrees
          
          --temp-limit tc[60-70]tm[105-115]
              enables both core and memory temperature limits

      --cpu-check
          Enables CPU verification of found shares before sending them to the pool

      --hashrate-avg <SECONDS>
          Hashrate averaging window in seconds. Default is 10.

      --cclock <FREQ1,FREQ2,...>
          Sets GPU core clock frequency offset in MHz
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          When dual or triple mining the value may be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

      --mclock <FREQ1,FREQ2,...>
          Sets GPU memory clock frequency offset in MHz
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          When dual or triple mining the value may be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

      --lock-cclock <FREQ1,FREQ2,...>
          Locks GPU core clock frequency to the given value in MHz
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          When dual or triple mining the value may be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

      --lock-mclock <FREQ1,FREQ2,...>
          Locks GPU memory clock frequency to the given value in MHz
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          When dual or triple mining the value may be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

      --pl <PL1,PL2,...>
          Set GPU power limit to the given value in Watts
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          When dual or triple mining the value may be prepended with
          the algorithm index `[<index>]`. Primary algorithm has index 1.

      --fan-control <FAN1,FAN2,...>
          Sets GPU fan control mode
          
          Can either force the GPU fans to work at a given static speed in %,
          or adjust the fan speed to keep the GPU temperature (core, memory, or both)
          at a desired level (auto-fan mode)
          
          Format:
              --fan-control N
                  set fan speed to `N`%
              --fan-control t:[Tc,Tm][Finit;Fmin-Fmax]
                  enables auto-fan with `Tc` and `Tm` core and memory temperatures
                  respectively, set initial fan speed to `Finit`% and limit the
                  fan speed range to [`Fmin`%,`Fmax`%] range.
                  every value is optional and can be set to underscore `_`
                  meaning the default value should be used.
          
          Comma-separated list of values can be used to set values per-GPU
          To skip a GPU, set the corresponding value to underscore `_`
          
          Examples:
          --fan-control 30
              sets GPU fan speed to 30%
          --fan-control t:[60;_][50;30-95]
              target core temp is 60 degrees, initial fan speed is 50%, 30%-95% range
          --fan-control t:[_;90][_;_-95]
              target memory temp is 90 degrees, max fan speed 95%
          --fan-control t:[70;100][_;_-_]
              adjust fan so that core temperature <= 70, memory temperature <= 100,
              with no restrictions on the fan speed

  -l, --log-file <LOG_FILE>
          Enables logging output of the miner to the specified log file

      --api-bind <IP:PORT>
          Enables HTTP API and binds it to the specified socket address
          
          Examples:
          --api-bind 127.0.0.1:5000
          --api-bind 0.0.0.0:5000

      --dns-over-https
          Enables pool DNS resolution using DNS-over-HTTPS (Cloudflare)

      --long-timestamps
          Enables milliseconds timestamps in the miner output

      --no-tui
          Disables terminal user interface (TUI)

      --no-watchdog
          Disables miner watchdog

  -h, --help
          Print help information (use `-h` for a summary)

  -V, --version
          Print version information
```

## Support
Discord: https://discord.gg/zKTgcGgc6k  
BitcoinTalk: https://bitcointalk.org/index.php?topic=5424675.0  
