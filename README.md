# kmodule

## Description

The module allows you to adjust the frequency of the functions used:
    - logging
    - argparse
    - load config from *.yaml or *.json

### Klog
`Klog.senesetive_filter` -- regxp. Filter which used to mask logging output.
`Klog.set_logging(self)` -- if logging level debug, set two handlers to save output to cli and to the file.

### kargs
`Kargs.add_defult_arguments` -- Set defaults arguments:
```
  -c, --config    `config file`
  -d, --debug     `debug mode`
  -l, --log       `set logfile`
```

### Config
Load config from *.yaml or *.json.
`sensetive_attributes` -- set attributes names which will be masked in output.
```python
# config.json
# {"one": 123, "two": 234, "three": 345}
k = Config('config.json', type='json', sensetive_attributes=['two', ])
print(k)
```
output will be:
```
one = 123
two = ***hidden***
three = 345
```


## Install

Still not published.

```shell
pip install git+https://github.com/Majorant/kmodule
```


## Usage example:

```python
import kmodule


CONFIG = 'config.yaml'


def get_args():
    """Get arguments from CLI
    Returns:
        class 'argparse.Namespace': args
    """
    args_parser = kmodule.Kargs(description='scrip descriptio')
    args_parser.add_defult_arguments()
    args_parser.add_argument('--compare-state', help='compare state', action='store_true', dest='compare_state')

    return args_parser.parse_args()

args = get_args()
config_file = args.config if args.config else CONFIG
conf = kmodule.Config(config_file, sensetive_attributes=['password', 'secret_key', 'token'])
log_level = 'debug' if args.debug else conf.log_level
kmodule.Klog(conf.log_file, log_level, senesetive_filter='[regex]+').set_logging()
```
