# Guía fundamental de Git y GitHub

Chuleta de los comandos que más uso para subir mis apuntes del Escritorio a GitHub.

> **Regla de oro:** cada carpeta del Escritorio es **su propio repositorio**.
> Los comandos siempre se ejecutan **dentro** de la carpeta (`cd ~/Escritorio/Nmap`), nunca en `~/Escritorio` a secas.

---

## 1. Lo del día a día: subir cambios (los 3 comandos)

Cuando agrego un `.txt`, un `.sh` o edito un apunte en una carpeta que **ya está en GitHub**:

```bash
cd ~/Escritorio/Nmap                  # 1. entrar a la carpeta
git add .                             # 2. marcar todos los cambios
git commit -m "Agrego tema 4"         # 3. guardar en el historial con un mensaje
git push                              # 4. subir a GitHub
```

| Comando | Qué hace |
|---|---|
| `git add .` | Prepara **todos** los archivos nuevos/modificados de la carpeta |
| `git add archivo.txt` | Prepara **solo** ese archivo |
| `git commit -m "mensaje"` | Guarda un punto en el historial (el mensaje explica qué hice) |
| `git push` | Sube los commits a GitHub |

---

## 2. Carpeta nueva → repositorio nuevo (solo la primera vez)

Si creo una carpeta nueva en el Escritorio (ej. `Wireshark`):

```bash
cd ~/Escritorio/Wireshark
git init                                                  # convierte la carpeta en repositorio
git add .
git commit -m "Subida inicial de apuntes"
gh repo create Wireshark --private --source . --push      # crea el repo PRIVADO en GitHub y sube
```

Desde ahí, ya funciona como la sección 1.

---

## 3. Ver qué está pasando

```bash
git status              # qué archivos son nuevos / cambiaron / están listos para commit
git diff                # qué líneas cambié exactamente (antes del add)
git log --oneline       # historial de commits, uno por línea
git remote -v           # a qué repositorio de GitHub está conectada la carpeta
```

- En `git status`: **rojo** = cambiado pero sin `add` · **verde** = listo para `commit`.
- Salir de `git log` o `git diff`: tecla **`q`**.

---

## 4. Deshacer cosas

```bash
git restore archivo.txt            # descartar cambios NO guardados de un archivo (vuelve al último commit) ⚠️ se pierden
git restore --staged archivo.txt   # quitar un archivo del "add" (no borra los cambios)
git commit --amend -m "nuevo msg"  # corregir el mensaje del ÚLTIMO commit (antes del push)
git log --oneline                  # ver versiones anteriores
git show <id>:carpeta/archivo.txt  # ver cómo era un archivo en un commit anterior
```

> En GitHub también se ve todo el historial: en el repo → **History** / **Commits**.

---

## 5. Ignorar archivos (que nunca se suban)

Crear un archivo `.gitignore` dentro de la carpeta con los nombres a ignorar:

```
# ejemplos
socket
*.ps
*.log
capturas/
```

⚠️ **Nunca subir:** contraseñas, tokens, claves API, capturas de pantalla o datos de terceros de un pentest real.

---

## 6. Sesión de GitHub (`gh`)

```bash
gh auth status          # ¿estoy conectado? ¿con qué cuenta?
gh auth logout          # cerrar sesión (recomendado cuando no la uso)
gh auth login --hostname github.com --git-protocol https --web   # volver a conectar
```

Al hacer login: copiar el código de 8 caracteres → Enter → pegarlo en https://github.com/login/device → **Authorize**.

**Si creo que me hackearon:** desde otro dispositivo → GitHub → *Settings → Applications → Authorized OAuth Apps → GitHub CLI → Revoke*.

---

## 7. Mis repositorios

```bash
gh repo list                         # ver todos mis repos
gh repo view --web                   # abrir en el navegador el repo de la carpeta actual
gh repo clone barArch99/Nmap         # descargar un repo (ej. en otro PC)
git pull                             # traer cambios de GitHub (si edité desde la web u otro PC)
```

> Si edito algo **desde la web de GitHub**, antes de seguir trabajando en el PC hago `git pull`.

---

## 8. Errores comunes

| Error | Causa | Solución |
|---|---|---|
| `fatal: not a git repository` | Estoy en la carpeta equivocada | `cd ~/Escritorio/<carpeta>` |
| `nothing to commit, working tree clean` | No hay cambios nuevos | Nada que hacer 🙂 |
| `rejected ... (fetch first)` al hacer push | En GitHub hay cambios que no tengo | `git pull` y luego `git push` |
| `Author identity unknown` | Git no sabe mi nombre | `git config --global user.name "barArch99"` y `user.email` |
| Pide usuario/contraseña al hacer push | La sesión de `gh` está cerrada | `gh auth login ...` (sección 6) |
| `gh: command not found` | `gh` no está instalado | `sudo apt install gh` (repo oficial de GitHub ya agregado) |

---

## 9. Resumen ultra corto

```bash
cd ~/Escritorio/<carpeta>
git status
git add .
git commit -m "qué hice"
git push
```
