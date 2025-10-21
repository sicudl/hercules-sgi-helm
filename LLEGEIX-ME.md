# Aplicació de changelogs HerculesCRUE/SGI

Aplicar els canvis necessaris als fitxers:

* charts/sgi-umbrella/Chart.yaml
* config/values.demo.yaml

```bash
helm repo update
helm package ./charts/sgi-umbrella/
# per actualitzar dependències dels charts al Chart.log generat !
helm package -u ./charts/sgi-umbrella/

# per comprovar configuració yaml que s'envia a k8s --dry-run
helm upgrade sgi sgi-umbrella-0.1.53.tgz --install --namespace sgi-demo -f ./config/values.demo.yaml --dry-run  > dry_run_0.1.53-upgrade.yml

helm upgrade sgi sgi-umbrella-0.1.53.tgz --install --namespace sgi-demo -f ./config/values.demo.yaml
```

## SGI Changelog 20230412

* [Aplicat 20230412](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20230412.md)
* [Aplicat SGI-AUTH 0_2_0](https://github.com/HerculesCRUE/SGI/blob/main/sgi-auth/changelog/v0_2_0.md)
* Nou grup *ADMINISTRADOR-SGI*

S'assigna a l'usuari *administrador-global* i permet gestionar configuracions genèriques de diversos mòduls (emails notificació, menú lateral Goliat del CSP, ....).

## SGI Changelog 20230721

* [Aplicat SGI 20230721](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20230721.md)
* [Aplicat SGI-AUTH 0_3_0](https://github.com/HerculesCRUE/SGI/blob/main/sgi-auth/changelog/v0_3_0.md)

* Nou grup *SYSDMN-CSP*

S'assigna a l'usuari *administrador-global* i *administrador-csp* i permet gestionar configuracions mòdul CSP (rols equip, rols socis i altres mestres).

## SGI Changelog 20231023

* [Aplicat 20231023](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20231023.md)
* [Aplicat SGI-AUTH 0_4_0](https://github.com/HerculesCRUE/SGI/blob/main/sgi-auth/changelog/v0_4_0.md)

## SGI Changelog 20240530

* [Aplicat 20240530](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20240530.md)

## SGI Changelog 20240607

* [Aplicat 20240607](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20240607.md)

## SGI Changelog 20240621

* [Aplicat 20240621](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20240621.md)

## SGI Changelog 20240715

* [Aplicat 20240715](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20240715.md)

## SGI Changelog 20241011

* [Aplicat 20241011](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20241011.md)

## SGI Changelog 20241210

* [Aplicat 20241210](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20241210.md)
* El upgrade a `sgi-esb: 0.7.0-um` no s'ha aplicat ja que és una customització del esb-sge per a la UM que s'especifica com a "Inclusión de campo "fondos europeos" y "programa" en el formly de alta proyecto SGE particular de la UMU." Amb aquest canvi ens deixa de funcionar l'assignació econòmica dels projectes.

## SGI Changelog 20250227

* [Aplicat 20250227](https://github.com/HerculesCRUE/SGI/blob/main/changelog/20250227.md)

## Ampliació de tamany del PVC de sgdoc

Si fem el canvi al fitxer values.demo.yaml de 16Gi a 32Gi

```yml
  # Service persistence configuration, when needed
  persistence:
    enabled: true
    storageClass: "default"
    existingClaim: "data-sgi-sgdoc"
    mountPath: /tmp/store
    accessModes:
      - ReadWriteOnce
    size: 32Gi
```

Dona un error:

```bash
hercules-sgi-helm$ helm upgrade sgi sgi-umbrella-0.1.53.tgz --install --namespace sgi-demo -f ./config/values.demo.yaml
Error: UPGRADE FAILED: cannot patch "sgi-sgdoc-service" with kind StatefulSet: StatefulSet.apps "sgi-sgdoc-service" is invalid: spec: Forbidden: updates to statefulset spec for fields other than 'replicas', 'ordinals', 'template', 'updateStrategy', 'persistentVolumeClaimRetentionPolicy' and 'minReadySeconds' are forbidden
```

Si fem el canvi des del **LENS** tot editant la configuració del *PVC* `data-sgi-sgdoc-service-0`

```yml
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 32Gi
```

en guardar es desecadena la ampliació del PVC i disc corresponent:

* MountVolume.NodeExpandVolume succeeded for volume "pvc-c225f25b-4cd1-4264-8422-821e60e69770" aks-defaultnpool-75499731-vmss000021
* Require file system resize of volume on node
* External resizer is resizing volume pvc-c225f25b-4cd1-4264-8422-821e60e69770
* waiting for an external controller to expand this PVC

## Neteja manual dels backups de la base de dades

1. Despleguem el pod amb ubuntu i el volum muntat a `/mnt/backup` amb permisos de lectura/escriptura

  ```bash
  kubectl apply -f ubuntu-backup-clean.yaml
  #pod/ubuntu-backup-clean created
  ```

2. Accedim al pod i eliminem els backup que no necessitem

3. Un cop fet neteja eliminem el pod

  ```bash
  kubectl delete pod ubuntu-backup-clean -n sgi-demo
  ```
