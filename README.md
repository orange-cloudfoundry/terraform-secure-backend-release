# terraform-secure-backend-release

A bosh release to deploy [terraform-secure-backend](https://github.com/orange-cloudfoundry/terraform-secure-backend).

## Prerequisite

Assumption has been made that you are using 
[BBL](https://github.com/orange-cloudfoundry/bosh-bootloader) with 
[cf-deployment](https://github.com/cloudfoundry/cf-deployment/) which include a deployed credhub

### Deploy It !

1. git clone this repo
2. include ops-file [/manifests/operators/cf/enable-tsb.yml](/manifests/operators/cf/enable-tsb.yml) from 
this repo to your Cloud Foundry deployment
3. Deploy with manifest in this repo [/manifests/tsb.yml](/manifests/tsb.yml)
```bash
bosh -d terraform-secure-backend deploy manifests/tsb.yml \
    -v director_name=<your bosh director name> \
    -v cf_deployment_name=cf
```
4. Retrieve user and password used for backend with credhub (the one in your bosh director): 
`credhub get --name=/<your bosh director name>/terraform-secure-backend/tsb_user`
5. *(Optional)* You probably want to make terraform-secure-backend available outside of your 
cf network, simply add ops-file [/manifests/operators/enable-cf-route-registrar.yml](/manifests/operators/enable-cf-route-registrar.yml) 
to your deployment. This will register a `tsb.((system_domain))` route to your gorouters.

### Ops file

- [/manifests/operators/enable-cef.yml](/manifests/operators/enable-cef.yml): Enable security event in common 
event format which will be stored at `/var/vcap/sys/log/terraform-secure-backend/security_events.log`
- [/manifests/operators/enable-cf-route-registrar.yml](/manifests/operators/enable-cf-route-registrar.yml): Register `tsb.((system_domain))` 
route to your gorouter (**Note**: gorouter will talk to it in full tls). Do not forget to set var `system_domain` when deploying
