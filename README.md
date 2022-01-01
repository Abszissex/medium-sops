

Requirements
Installed "sops" -> https://github.com/mozilla/sops/releases
Installed gcloud SDK
---
Run 
`terraform apply`

Check created key rings in Google Console
key_ring.png
Can also be verified via `gcloud kms keys list --location global --keyring sops-demo`
cli_key_ring.png

TODO: put `projects/pascal-sandbox-1112/locations/global/keyRings/sops-demo/cryptoKeys/my-encryption-key` into env variable for readability
Decrypt a file
`sops --encrypt --gcp-kms projects/pascal-sandbox-1112/locations/global/keyRings/sops-demo/cryptoKeys/my-encryption-key secret.yaml > secret.enc.yaml`

-> show secret.enc.yaml
Issue: now everything is encrypted and it's bad for maintenance
-> `--encrypted-regex`
`sops --encrypt --encrypted-regex '^(data)$' --gcp-kms projects/pascal-sandbox-1112/locations/global/keyRings/sops-demo/cryptoKeys/my-encryption-key secret.yaml > secret_partial.enc.yaml`

-> Decrypting
sops --decrypt --gcp-kms projects/pascal-sandbox-1112/locations/global/keyRings/sops-demo/cryptoKeys/my-encryption-key secret_partial.enc.yaml

Talk about `--in-place`
sops --encrypt --encrypted-regex '^(data)$' --in-place --gcp-kms projects/pascal-sandbox-1112/locations/global/keyRings/sops-demo/cryptoKeys/my-encryption-key secret_o.yaml

