# ![Gawp](http://praegress.us/gawp-logo.png)
A simple, configurable, file watching, job execution tool implemented in Go.

**gawp**<br>ɡɔːp/<br>*verb, British, informal*<br>stare openly in a stupid or rude manner.

## Installation
    go get gopkg.in/fsnotify.v1
    go get gopkg.in/yaml.v2
    go get github.com/martingallagher/gawp

The following assumes your Go $GOPATH/bin is on your $PATH environmental variable (`export PATH=$PATH:$GOPATH/bin`).

## Configuration
By default Gawp attempts to read `.gawp` in the active directory. The file format is [YAML](http://www.yaml.org/).

The configuration file location can be set using the `-config my.conf` command-line flag.

**Example .gawp file:**

```yaml
recursive: true           # Watch directories recursively, default: true
verbose: false            # Verbose logging, default: false
workers: 4                # Number of concurrent workers (high numbers can thrash IO), default: number CPUs / 2 (minimum 1)
#logfile: gawp.log        # Gawp logfile, default: stdout

write, create, rename:    # Actionable events (supported: create, write, rename, remove, chmod), executed sequentially
  (?i)([a-z]+)\.src\.js$: # Rules are regular expression strings (https://code.google.com/p/re2/wiki/Syntax)
  - java -jar ~/compiler.jar -O=ADVANCED --language_in=ECMASCRIPT5_STRICT --formatting=SINGLE_QUOTES --define='DEBUG=false' --js_output_file=scripts/$1.js $file

  (?i)[a-z]+\.scss:
  - compass compile --boring --time -s compressed --css-dir styles/ $file
  - echo HELLO DENNIS!    # Rules can have multiple commands; output is written as-is to the Gawp log

create:
  .*:
  - echo created $file    # Rule submatches and file path can be accessed via $1, $2 ... $n (nth submatch) and $file

remove:
  .*:
  - echo removed $file
```

Configuration defaults fulfil most user's requirements, resulting in a config file that just defines rules:

```yaml
write, create, rename:
  (?i)([a-z]+)\.src\.js$:
  - java -jar ~/compiler.jar -O=ADVANCED --language_in=ECMASCRIPT5_STRICT --formatting=SINGLE_QUOTES --define='DEBUG=false' --js_output_file=scripts/$1.js $file

  (?i)[a-z]+\.scss:
  - compass compile --boring -s compressed --css-dir styles/ $file

create:
  .*:
  - echo created $file

remove:
  .*:
  - echo removed $file
```

# Usage
Assuming correctly configured `web/assets/.gawp` file: `cd web/assets/ && gawp`

# Contributions
Bug fixes and feature requests welcome.

# Contributors
- [Martin Gallagher](http://martingallagher.com/)