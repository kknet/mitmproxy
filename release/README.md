# Release Checklist

Make sure run all these steps on the correct branch you want to create a new release for!
- Verify `mitmproxy/version.py`
- Update CHANGELOG
- Verify that all CI tests pass
- Tag the release and push to Github
  - For alphas, betas, and release candidates, use lightweight tags.  
    This is necessary so that the .devXXXX counter does not reset.
  - For final releases, use annotated tags.  
    This makes the .devXXXX counter reset.
- Wait for tag CI to complete

## GitHub Release
- Create release notice on Github [here](https://github.com/mitmproxy/mitmproxy/releases/new)
- Attach all files from the new release folder on https://snapshots.mitmproxy.org

## PyPi
- Upload wheel to pypi: `twine upload <mitmproxy-...-.whl`

## Docker
- Update docker-releases repo
  - Create a new branch based of master for major versions.
  - Update the dependencies in [alpine/requirements.txt](https://github.com/mitmproxy/docker-releases/commit/3d6a9989fde068ad0aea257823ac3d7986ff1613#diff-9b7e0eea8ae74688b1ac13ea080549ba)
    * Creating a fresh venv, pip-installing the new wheel in there, and then export all packages:
    * `virtualenv -ppython3.5 venv && source venv/bin/activate && pip install mitmproxy && pip freeze`
  - Tag the commit with the correct version
    * `2.0.0` for new major versions
    * `2.0.2` for new patch versions
    * `2.0` always points to the latest patch version of the `2.0.x` series (update tag + force push)
- Update `latest` tag [here](https://hub.docker.com/r/mitmproxy/mitmproxy/~/settings/automated-builds/)

After everything is done, you might want to bump the version on master in [https://github.com/mitmproxy/mitmproxy/blob/master/mitmproxy/version.py](mitmproxy/version.py) if you just created a major release.
