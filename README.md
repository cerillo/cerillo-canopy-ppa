# cerillo-aptly

Storage for .deb packages and instructions on deployment

## Initial Setup
First timers must
1. Install `aptly`
  `brew install aptly`
2. Initialize their local debian repository
  `aptly repo create -distribution=stable -component=main canopy-release`
3. Import the gpg private key `gpg --import cerillo-ppa-key.asc` (not saved in this repository, get it from someone)

## To update the repository snapshot
1. Add any new releases to `dists/`
2. Add all releases to the repository (ok to do multiple times)
  `aptly repo add canopy-release dists`
3. (optional) sanity check `aptly repo show -with-packages canopy-release`

## Publishing
1. Remove old snapshot `aptly publish drop stable; aptly snapshot drop canopy-snapshot`
1. Create a new snapshot `aptly snapshot create canopy-snapshot from repo canopy-release`
  Note: The name of the snapshot doesn't actually matter, but it's helpful to name it after the release version if you want to look at old local snapshots
2. Publish `aptly -architectures=all -gpg-key=canopy-ppa@cerillo.com publish snapshot canopy-snapshot`
3. Copy the local repo to here `rm -r public;cp -r ~/.aptly/public .`
4. `git push` and it'll be published to Github Pages shortly.