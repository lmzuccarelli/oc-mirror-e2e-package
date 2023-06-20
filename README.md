# Overview

This is a hand crafted release payload used in oc-mirror-e2e testing of release images

## Creating a release image

Use the existing directory in oci-testing called ```oc-mirror-e2e-package```

The current tar file where the image-releases file lies is a24e676ee96c3b345e2441b8f55e03c159ff154c79c5dd9bf46b8e2e229456b3
viewed via the index.json -> manifest -> tar.gaz

- untar it
- open and update release-images
- save it
- tar -zcvf a24e676ee96c3b345e2441b8f55e03c159ff154c79c5dd9bf46b8e2e229456b3 release-images
- update the manifest blob a5be0b5297398cfcc4d0663c64ff32bd087f38bf5c474ed998decd72a5b01cd4 (update size) 
- sha256sum file > checksum
- cat checksum
- use checksum to rename file
- skopeo copy oci://home/lzuccarelli/Projects/oc-mirror-e2e-package/release-payload docker://localhost:5000/test/e2e-test-release:latest
- skopeo inspect docker://localhost:5000/test/e2e-test-release:latest (get digest)
- skopeo copy oci://home/lzuccarelli/Projects/oci-testing/oc-mirror-e2e-release  docker://localhost:5000/test/e2e-release@sha256<digest>
- update cincinnati.json (oc-mirror/test/e2e/graph)

N.B. Update blob digest

```
skopeo copy oci://home/lzuccarelli/Projects/oci-testing/oc-mirror-e2e-release docker://localhost:5000/test/e2e-test-release:latest --dest-tls-verify=false 
Getting image source signatures
Copying blob 93809c7db2a0 done  
Copying blob 766eb4e6e310 done  
Copying blob f0f4937bc70f done  
Copying blob 833de2b0ccff done  
Copying blob 7ef49f1c6ee7 done  
Copying blob 2c22ea1011b9 done  
Copying blob 9e0819731a37 done  
FATA[0000] writing blob: Patch "http://localhost:5000/v2/test/e2e-test-release/blobs/uploads/374e662e-f448-4167-a61d-af5680ec8520?_state=8DDgVoUJqJO8fWdeNfPX76hKpnidm6SiYTmLSSFD_Sl7Ik5hbWUiOiJ0ZXN0L2UyZS10ZXN0LXJlbGVhc2UiLCJVVUlEIjoiMzc0ZTY2MmUtZjQ0OC00MTY3LWE2MWQtYWY1NjgwZWM4NTIwIiwiT2Zmc2V0IjowLCJTdGFydGVkQXQiOiIyMDIzLTAxLTI3VDAzOjQxOjU3Ljk2MjMxMTg0NloifQ%3D%3D": happened during read: Digest did not match, expected sha256:9e0819731a372f9a22156b3151f41640646a29cba8c2d77e4a7f73bcab434cc1, got sha256:780d8ca21cad007ed9fdb461a7da8819c96e4dbc14255717a54c7d9fc128b37b 

```
i.e 

```
mv 9e0819731a372f9a22156b3151f41640646a29cba8c2d77e4a7f73bcab434cc1 780d8ca21cad007ed9fdb461a7da8819c96e4dbc14255717a54c7d9fc128b37b
```

N.B the new image is now 780d8ca21cad007ed9fdb461a7da8819c96e4dbc14255717a54c7d9fc128b37b

update 2062ca6881f621409785d655fb4fcd15b681efc6449581c8a0dccc14ddf8e2c5 with the new image digest

Remember to clean out local registry var/lib/registry and restart it




