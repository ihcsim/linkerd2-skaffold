# -*- mode: Python -*-

trigger_mode(TRIGGER_MODE_MANUAL)
load("./bin/_tilt", "components", "image_tag", "images", "linkerd_yaml", "settings")

#default_registry(settings.get("default_registry"))
allow_k8s_contexts(settings.get("allow_k8s_contexts"))

k8s_yaml(linkerd_yaml())

for component in components:
  k8s_resource(
    component["name"],
    new_name=component["new_name"],
    port_forwards=component["port_forwards"] if "port_forwards" in component else [],
    extra_pod_selectors=component["labels"],
  )

for image in images:
  custom_build(
    image["image"],
    "./bin/docker-build-%s && docker tag %s:%s $EXPECTED_REF" % (image["new_name"], image["image"], image_tag()),
    image["deps"],
    tag=image_tag()
  )
