# mkwheelhouse

Amazon S3 wheelhouse generator.

Wheels are the latest standard in distributing binary for Python. Wheels cut
down scipy's installation time from 15 minutes to 15 seconds.

> [Wheel documentation][wheel-docs]

## Usage

Generate wheels for all listed `PACKAGE`s and their dependencies, then upload
them to Amazon S3 `BUCKET`:

```bash
$ mkwheelhouse BUCKET [PACKAGE...]
```

Then install with pip like usual, but preferring generated wheels:

```bash
$ pip install --find-links BUCKET/index.html PACKAGE
```

### Additional options

* `-h`, `--help`

  Print usage information and exit.

* `-r`, `--requirement REQUIREMENTS_FILE`

  Also include packages (and their dependencies) from the pip requirements file
  REQUIREMENTS_FILE. Can be specified multiple times and combined with
  positional PACKAGE arguments.

* `-e`, `--exclude WHEEL_FILENAME`:

  Don't upload built wheel with filename WHEEL_FILENAME. Note this is the
  final wheel filename, like `argparse-1.3.0-py2.py3-none-any.whl`, *not* the
  bare package name.

  Specifying an exclusion will not remove pre-existing built wheels from
  S3; you'll have to remove those wheels from the bucket manually.

## Notes

* Python 2 and 3

* Set a [bucket policy to make all objects publicly accessible][public-policy]
  or Pip won't  be able to download wheels from your wheelhouse.

[public-policy]: http://docs.aws.amazon.com/AmazonS3/latest/dev/AccessPolicyLanguage_UseCases_s3_a.html
[wheel-docs]: http://wheel.readthedocs.org/en/latest/
