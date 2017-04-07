# api documentation for  [node-env-file (v0.1.8)](https://github.com/grimen/node-env-file)  [![npm package](https://img.shields.io/npm/v/npmdoc-node-env-file.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-node-env-file) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-node-env-file.svg)](https://travis-ci.org/npmdoc/node-npmdoc-node-env-file)
#### Parse and load environment files (containing ENV variable exports) into Node.js environment, i.e. `process.env`.

[![NPM](https://nodei.co/npm/node-env-file.png?downloads=true)](https://www.npmjs.com/package/node-env-file)

[![apidoc](https://npmdoc.github.io/node-npmdoc-node-env-file/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-node-env-file_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-node-env-file/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-node-env-file/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-node-env-file/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Jonas Grimfelt",
        "email": "grimen@gmail.com"
    },
    "bin": {},
    "bugs": {
        "url": "https://github.com/grimen/node-env-file/issues"
    },
    "contributors": [],
    "dependencies": {},
    "description": "Parse and load environment files (containing ENV variable exports) into Node.js environment, i.e. 'process.env'.",
    "devDependencies": {
        "chai": "latest",
        "cover": "latest",
        "glob": "~3.2.0",
        "mocha": "latest",
        "temp": "~0.8.0"
    },
    "directories": {},
    "dist": {
        "shasum": "fccb7b050f735b5a33da9eb937cf6f1ab457fb69",
        "tarball": "https://registry.npmjs.org/node-env-file/-/node-env-file-0.1.8.tgz"
    },
    "engines": {
        "node": ">= 0.8.0"
    },
    "gitHead": "0eb1bb384ba414366a65729e8f8256f1230d1360",
    "homepage": "https://github.com/grimen/node-env-file",
    "keywords": [
        "process",
        "env",
        "file",
        "files",
        ".env",
        "ENV",
        "process.env",
        "parse",
        "load",
        "export",
        "exports"
    ],
    "licenses": [
        {
            "type": "MIT",
            "url": "https://github.com/grimen/node-env-file/blob/MIT-LICENSE"
        }
    ],
    "maintainers": [
        {
            "name": "grimen",
            "email": "grimen@gmail.com"
        }
    ],
    "name": "node-env-file",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/grimen/node-env-file.git"
    },
    "scripts": {
        "test": "make test",
        "test-ci": "make test-ci"
    },
    "version": "0.1.8"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module node-env-file](#apidoc.module.node-env-file)
1.  boolean <span class="apidocSignatureSpan">node-env-file.</span>log
1.  boolean <span class="apidocSignatureSpan">node-env-file.</span>overwrite
1.  boolean <span class="apidocSignatureSpan">node-env-file.</span>raise
1.  [function <span class="apidocSignatureSpan">node-env-file.</span>sync (env_file, options)](#apidoc.element.node-env-file.sync)
1.  object <span class="apidocSignatureSpan">node-env-file.</span>data
1.  object <span class="apidocSignatureSpan">node-env-file.</span>logger



# <a name="apidoc.module.node-env-file"></a>[module node-env-file](#apidoc.module.node-env-file)

#### <a name="apidoc.element.node-env-file.sync"></a>[function <span class="apidocSignatureSpan">node-env-file.</span>sync (env_file, options)](#apidoc.element.node-env-file.sync)
- description and source-code
```javascript
sync = function (env_file, options) {
    options = options || {}

    if (typeof options.verbose === 'undefined') {
        options.verbose = module.exports.verbose
    }

    if (typeof options.overwrite === 'undefined') {
        options.overwrite = module.exports.overwrite
    }

    if (typeof options.raise === 'undefined') {
        options.raise = module.exports.raise
    }

    module.exports.logger = options.logger || module.exports.logger

    if (typeof env_file !== 'string') {
        if (options.raise) {
            throw new TypeError("Environment file argument is not a valid 'String': " + env_file)
        } else {
            if (options.verbose && module.exports.logger) {
                module.exports.logger.error('[ENV]: ERROR Environment file argument is not a valid 'String':', process.env.ENV_FILE
)
            }
            return {}
        }
    }

    try {
        env_file = process.env.ENV_FILE = path.resolve(env_file) || process.env.ENV_FILE
    } catch (err) {
        if (options.raise) {
            throw new TypeError("Environment file path could not be resolved: " + err)
        } else {
            if (options.verbose && module.exports.logger) {
                module.exports.logger.error('[ENV]: ERROR Environment file path could not be resolved:', process.env.ENV_FILE)
            }
            return {}
        }
    }

    module.exports.data = module.exports.data || {}
    module.exports.data[env_file] = {}

    if (options.verbose && module.exports.logger) {
        module.exports.logger.info('[ENV]: Loading environment:', env_file)
    }

    if (fs.existsSync(env_file)) {
        var lines

        try {
            lines = (fs.readFileSync(env_file, 'utf8') || '')
                .split(/\r?\n|\r/)
                .filter(function (line) {
                    return /\s*=\s*/i.test(line)
                })
                .map(function (line) {
                    return line.replace('exports ', '')
                })

        } catch (err) {
            if (options.raise) {
                throw new TypeError("Environment file could not be read: " + err)
            } else {
                if (options.verbose && module.exports.logger) {
                    module.exports.logger.error('[ENV]: ERROR Environment file could not be read:', env_file)
                }
                return {}
            }
        }

        module.exports.lines = module.exports
        module.exports.lines.comments = []
        module.exports.lines.variables = []

        var is_comment

        lines.forEach(function(line) {
            is_comment = /^\s*\#/i.test(line) // ignore comment lines (starting with #).

            if (is_comment) {
                module.exports.lines.comments.push(line)

                if (options.verbose && module.exports.logger) {
                    module.exports.logger.info('[ENV]: Ignored line:', line)
                }

            } else {
                module.exports.lines.variables.push(line)

                var key_value = line.match(/^([^=]+)\s*=\s*(.*)$/)

                var env_key = key_value[1]

                // remove ' and " characters if right side of = is quoted
                var env_value = key_value[2].match(/^(['"]?)([^\n]*)\1$/m)[2]

                // overwrite already defined 'process.env.*' values?
                if (!!options.overwrite) {
                    module.exports.data[env_file][env_key] = env_value

                    if (options.verbose && module.exports.logger && module.exports.data[env_file][env_key] !== env_value) {
                        module.exports.logger.info('[ENV]: Overwritten ', module.exports.data[env_file][env_key], ' => ', env_value
)
                    }

                } else {
                    module.exports.data[env_file][env_key] = process.env[env_key] || env_value
                }

                process.env[env_key] = module.exports.data[env_file][env_key]

                if (options.verbose && module.exports.logger) {
                    module.exports.logger.info('[ENV]:', module.exports.data[en ...
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
