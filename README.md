# TLembro — fonte (source) do AltStore Classic

Esta pasta tem tudo pronto para publicar o TLembro no **AltStore Classic**:

```
altstore/
├── source.json        ← a sua "source" (o que a pessoa adiciona no AltStore)
├── icon.png           ← ícone 1024×1024 do app
├── screenshots/       ← (opcional) coloque aqui prints em PNG
└── TLembro.ipa        ← o app (gerado; veja abaixo)
```

> Troque **`SEU-USUARIO`** por seu usuário do GitHub em todos os lugares.

---

## Passo 1 — Criar o repositório e subir os arquivos

Com o [GitHub CLI (`gh`)](https://cli.github.com) autenticado:

```bash
cd altstore
git init -b main
git add source.json icon.png screenshots
git commit -m "TLembro AltStore source"
gh repo create tlembro-altstore --public --source=. --push
```

## Passo 2 — Subir o IPA como Release

O `.ipa` é grande para o repositório comum, então vai como **Release asset**:

```bash
gh release create v1.0 TLembro.ipa --repo SEU-USUARIO/tlembro-altstore --title "TLembro 1.0" --notes "Primeira versão"
```

Isso cria a URL do IPA:
`https://github.com/SEU-USUARIO/tlembro-altstore/releases/download/v1.0/TLembro.ipa`
(ela já está no `downloadURL` do `source.json`).

## Passo 3 — Publicar a source (GitHub Pages)

```bash
gh api -X POST repos/SEU-USUARIO/tlembro-altstore/pages -f "source[branch]=main" -f "source[path]=/" 2>/dev/null || true
```

Ou ative em **Settings → Pages → Branch: main /(root)** pelo site.
A URL da sua source fica:

- Via raw (funciona na hora): `https://raw.githubusercontent.com/SEU-USUARIO/tlembro-altstore/main/source.json`
- Via Pages: `https://SEU-USUARIO.github.io/tlembro-altstore/source.json`

## Passo 4 — Ajustar as URLs no source.json

Abra o `source.json` e troque `SEU-USUARIO` pelas URLs reais do seu repositório (ícone, screenshots, downloadURL).

## Passo 5 — Instalar/compartilhar

Quem quiser instalar: **AltStore → Browse → Sources → +** e cola a URL da source. O TLembro aparece; toca em **GET**.

---

## Atualizar para uma próxima versão

1. Suba a versão no Xcode (`CFBundleVersion` e, se quiser, `CFBundleShortVersionString`).
2. Gere o novo IPA.
3. `gh release create v1.1 TLembro.ipa ...`
4. Adicione uma **nova entrada em `versions`** no `source.json` (a mais nova no topo) e dê `git push`.

---

## ⚠️ Lembretes importantes

- **Limite de 7 dias:** quem instalar com Apple ID grátis precisa renovar pelo AltServer. Conta paga = 1 ano.
- **Widget / App Group:** só funciona para quem instalar com **conta paga**. Com conta grátis, o widget é removido na reassinatura (o resto do app funciona normal).
- O IPA aqui é **não assinado de propósito** — o AltStore reassina com o Apple ID de quem instala.

Guia completo (incluindo o caminho AltStore PAL): `../PUBLICAR_NA_ALTSTORE.md`.
