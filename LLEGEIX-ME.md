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
helm upgrade sgi sgi-umbrella-0.1.50.tgz --install --namespace sgi-demo -f ./config/values.demo.yaml --dry-run  > dry_run_0.1.50-upgrade.yml

helm upgrade sgi sgi-umbrella-0.1.50.tgz --install --namespace sgi-demo -f ./config/values.demo.yaml
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
