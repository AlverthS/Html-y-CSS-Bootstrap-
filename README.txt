# F1 Website · Versión Bootstrap (Tema Deportivo)

Este paquete contiene una versión mejorada con **Bootstrap 5** del HTML original, con un diseño **deportivo** (F1).

## Contenido
- `index.html` — página principal lista para abrir en el navegador.
- `assets/style.css` — estilos personalizados para darle look F1 (negro/rojo, bandera a cuadros).
- `Abrir-EN-Windows.bat` — abre `index.html` en el navegador por defecto (opcional).
- `README.txt` — este archivo.

## ¿Y el .EXE?
En este entorno no puedo compilar un `.exe` real aquí mismo. Te dejo dos opciones rápidas para tener un ejecutable en Windows:

### Opción A — WebView2 (C# / .NET) en 2–3 minutos
1. Copia el siguiente código C# en un archivo `F1Launcher.cs` (está al final de este README).
2. En Windows, abre **Developer PowerShell** de Visual Studio o usa `dotnet`:
   ```bash
   dotnet new console -n F1Launcher
   cd F1Launcher
   dotnet add package Microsoft.Web.WebView2
   Reemplaza Program.cs por el contenido de F1Launcher.cs
   dotnet publish -c Release -r win-x64 -p:PublishSingleFile=true -p:IncludeAllContentForSelfExtract=true
   ```
3. Copia la carpeta `f1_bootstrap` junto al `.exe` publicado. Al ejecutarlo abrirá el sitio embebido.

### Opción B — Electron
1. `npm init -y && npm i electron`
2. Crea `main.js` que apunte a `index.html` y compila con `electron-builder` (o usa `electron-packager`).

> Si quieres, te paso un script listo para **Electron** o **C# WebView2** personalizado a tu nombre/logo.

## Cómo usar
- Doble clic en `index.html` o ejecuta `Abrir-EN-Windows.bat`.
- Navegación: usa el menú (Home, Pilotos, Ingenieros, Acerca). Tablas responsivas y tema deportivo.

---

## F1Launcher.cs (plantilla WebView2)

using System;
using System.IO;
using System.Reflection;
using System.Windows.Forms;
using Microsoft.Web.WebView2.WinForms;

class F1Launcher : Form
{
    WebView2 webview;
    public F1Launcher()
    {
        Text = "F1 Website";
        Width = 1100; Height = 700;
        webview = new WebView2 { Dock = DockStyle.Fill };
        Controls.Add(webview);
        Load += async (_, __) =>
        {
            await webview.EnsureCoreWebView2Async(null);
            // Ajusta la ruta si cambias la estructura
            var htmlPath = Path.Combine(AppContext.BaseDirectory, "f1_bootstrap", "index.html");
            webview.CoreWebView2.Navigate(new Uri(htmlPath).AbsoluteUri);
        };
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new F1Launcher());
    }
}