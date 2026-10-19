<div align="center">

<img src="https://nodeaec.com.br/assets/img/icon/logo-og.png" alt="Node.aec Logo" width="140" />

# Node.aec

**Plugins, families, and templates for your BIM project.**

[Website](https://nodeaec.com.br) • [Catalog](https://nodeaec.com.br/products) • [Instagram](https://www.instagram.com/node.aec) • [Contact](mailto:dev@nodeaec.com.br)

[![Revit Compatibility](https://img.shields.io/badge/Autodesk%20Revit-2023--2027-0696D7?style=flat-square&logo=autodesk)](https://github.com/nodeaec/revit-connector)
[![Platform](https://img.shields.io/badge/.NET-4.8%20%7C%208.0%20%7C%2010.0-512BD4?style=flat-square&logo=dotnet)](https://github.com/nodeaec/templates-revit)
[![Licensing Security](https://img.shields.io/badge/Security-Ed25519%20%2B%20DPAPI-2ea44f?style=flat-square)](https://github.com/nodeaec/licensing-lite)
[![Ecosystem](https://img.shields.io/badge/Ecosystem-BIM%20Marketplace-F2EC94?style=flat-square&labelColor=1a1a1a)](https://nodeaec.com.br)

---

</div>

## 🌐 About Node.aec

**Node.aec** is the digital distribution, marketplace, and licensing platform for the Architecture, Engineering, and Construction (AEC) industry. We bridge the gap between BIM content creators and project designers:

- ⚡ **Plugins & Automation**: Tools that eliminate repetitive modeling and documentation tasks directly from your BIM software.
- 📐 **Libraries & Families**: Production-ready, validated BIM objects and component catalogs ready to insert into projects.
- 📋 **Templates & Standards**: Project standards for architecture, structure, and MEP ready to use.
- 📚 **Guides & E-books**: Technical documentation, workflow guidelines, and BIM process management.
- 🤝 **Connections & Ecosystem**: A curated network uniting project owners, specialized consultancies, and AEC talent.

---

## 🏛️ Developer Infrastructure: Hub & Micro-Gate Architecture

Node.aec provides BIM creators with turnkey licensing and distribution infrastructure to build sustainable software businesses without developing custom licensing servers, machine DRM, or payment pipelines.

The platform decouples workstation license management from individual add-ins:

```mermaid
flowchart TD
    subgraph Cloud["Node.aec Cloud Platform"]
        Auth["OAuth 2.0 &amp; Licensing Services"]
    end

    subgraph Revit["Autodesk Revit (Workstation)"]
        subgraph Hub["Node.aec Connector (Central Hub)"]
            direction TB
            SSO["Browser SSO (RFC 8252 Loopback)"]
            Lease["Master Entitlements Lease (Ed25519)"]
            Store["DPAPI Protected Storage (30d Grace)"]
            Ribbon["Shared Ribbon Tab Deduplication"]
        end

        subgraph Plugins["Partner / Internal Add-ins"]
            direction TB
            Cmd["IExternalCommand.Execute()"]
            Gate["NodeAec.Licensing.Lite (Micro-Gate)"]
            Verdict{"IsLicensed?"}
            Run["Execute Feature"]
            Prompt["Prompt Activation / Open Connector"]

            Cmd --> Gate
            Gate --> Verdict
            Verdict -->|"Yes"| Run
            Verdict -->|"No"| Prompt
        end

        Gate -->|"Fast Local Verification (&lt; 1 ms)"| Lease
        Prompt -.->|"Gate.OpenConnector()"| Hub
    end

    Auth <-->|"Sync Entitlements &amp; Leases"| Hub
```

---

## 📦 Repositories & Tooling

| Repository | Description | Key Technologies |
|---|---|---|
| [`revit-connector`](https://github.com/nodeaec/revit-connector) | Central desktop governance and license manager add-in for **Autodesk Revit 2023–2027**. Implements Browser SSO (RFC 8252 loopback OAuth 2.0), DPAPI-encrypted leases, Ed25519 cryptographic verification, and shared ribbon tab management. | C#, WPF, Revit API, RFC 8252, Ed25519 |
| [`licensing-lite`](https://github.com/nodeaec/licensing-lite) | `NodeAec.Licensing.Lite` — ultra-fast offline license gate NuGet package. Validates signed leases in 3 lines of code with zero network overhead (`< 1 ms`) and fail-closed design. | NuGet, .NET Standard / .NET Core, Ed25519 |
| [`templates-revit`](https://github.com/nodeaec/templates-revit) | `dotnet new nodeaec-revit-plugin` — Official template for scaffolding licensed Revit add-ins with multi-targeting (2023–2027), build matrix, and automated installer generation. | `dotnet new`, MSBuild, Inno Setup |
| [`revit-sample-plugin`](https://github.com/nodeaec/revit-sample-plugin) | Reference Revit add-in illustrating end-to-end license gating and UX integration with `NodeAec.Licensing.Lite`. | C#, Revit API, Sample Integration |

---

## 🚀 Building a Licensed Revit Add-in in Minutes

Install the official template and scaffold your plugin:

```powershell
# 1. Install the Node.aec Revit plugin template
dotnet new install https://github.com/nodeaec/templates-revit.git

# 2. Scaffold a new licensed add-in project
dotnet new nodeaec-revit-plugin -n MyPlugin --ProductSlug my-plugin --AddInId (New-Guid)

# 3. Build for your target Revit version (e.g. Revit 2026)
cd MyPlugin
dotnet build -p:RevitYear=2026
```

Integrate license gating in your external command in just 3 lines:

```csharp
using NodeAec.Licensing;

var gate = Gate.Validate("my-plugin");
if (!gate.IsLicensed)
{
    TaskDialog.Show("License Required", gate.Message);
    Gate.OpenConnector(); // Opens the Node.aec Hub to activate or resolve issues
    return Result.Cancelled;
}

// Your feature execution here...
```

---

## 📬 Contact & Community

- 🌐 **Platform**: [nodeaec.com.br](https://nodeaec.com.br)
- 🛒 **BIM Catalog**: [nodeaec.com.br/products](https://nodeaec.com.br/products)
- 📸 **Instagram**: [@node.aec](https://www.instagram.com/node.aec)
- 💬 **Developer Inquiries**: [dev@nodeaec.com.br](mailto:dev@nodeaec.com.br)
- 🐛 **Issues & Feedback**: Open an issue in the respective [repository tracker](https://github.com/orgs/nodeaec/repositories)

<div align="center">
<sub>Built with precision for the AEC industry • Node.aec</sub>
</div>
