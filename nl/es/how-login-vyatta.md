---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Cómo iniciar sesión en Vyatta

Se puede acceder a Brocade NFV (virtualización de funciones de red) mediante una GUI web o directamente con un inicio de sesión SSH.

## Autenticarse en la red privada de SoftLayer

1. Especifique la dirección IP privada en la pantalla **Detalles** (menú **Red > Dispositivos de pasarela**) en el navegador con un prefijo https:// (por ejemplo, https://10.54.207.131/). Tenga en cuenta que es posible que reciba un error de certificado porque el dispositivo no utiliza un certificado público. Acepte el error para ir a la página web.

2. Especifique las credenciales del separador **Contraseña** ubicado en el menú **Dispositivos** en el portal web para el usuario **vyatta**.  Irá al panel de control de Brocade.
