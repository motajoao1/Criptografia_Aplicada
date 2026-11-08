# Criptografia Aplicada — XYZW Tecnologia

Trabalho 01 de MATC99 — Segurança e Auditoria de Sistemas de Informação (UFBA).

Os documentos, as autoridades certificadoras e as chaves deste projeto são fictícios
e foram criados para testes. **Nunca utilize as chaves privadas fornecidas em produção.**

## Requisitos

- Python 3.9 ou superior e `cryptography` (`python -m pip install cryptography`).
- OpenSSL 3.x e Bash, disponíveis em Linux, macOS ou WSL/Git Bash no Windows.
- Ambiente documentado no relatório: Python 3.11.9, `cryptography` 50.0.2 e OpenSSL 3.5.5.

## Parte I — AES-256-GCM

No terminal, a partir da raiz desta pasta:

```bash
cd parte1
python3 aes_gcm_demo.py doc_teste.txt
```

No Windows, se `python3` não estiver disponível, use `python`.

O script lê `doc_teste.txt`, gera `doc_teste.enc` no formato
`nonce (12 bytes) || dados cifrados || tag (16 bytes)` e executa os sete testes
T1–T7. Também grava uma nova `chave_TESTE.bin` para reprodução local.

**Atenção:** a cada execução, a chave, o nonce e o arquivo cifrado são
substituídos. Os valores hexadecimais deixam de coincidir com o log original;
os resultados esperados permanecem: T1 aprovado e T2–T7 rejeitados com `InvalidTag`.

## Parte II — Assinaturas e certificados

Se você terminou a Parte I e ainda está na pasta `parte1/`, volte à raiz
do trabalho com `cd ..`. A partir da raiz, execute:

```bash
cd parte2
bash parte2.sh
```

O comando recria as pastas `pki/` e `casos/` com uma infraestrutura de teste
nova, monta os cenários A, B e C e verifica assinatura e cadeia de certificados.
**Não execute sobre um conjunto de resultados que deseje preservar sem fazer cópia.**

Para repetir apenas a verificação do caso A, sem recriar os arquivos:

```bash
bash verificar.sh casos/A/documento.txt casos/A/documento.sig casos/A/signatario.crt pki/ca_confiavel.crt
```

Repita trocando `A` por `B` ou `C` para os outros casos. Os resultados esperados são:

| Caso | Assinatura | Cadeia | Decisão |
| --- | --- | --- | --- |
| A — original legítimo | OK | OK | ACEITAR |
| B — documento adulterado | FALHA | OK | REJEITAR |
| C — certificado não confiável | OK | FALHA | REJEITAR |

Para verificar os três casos sem as chaves privadas, bastam
`casos/*/documento.txt`, `casos/*/documento.sig`,
`casos/*/signatario.crt` e `pki/ca_confiavel.crt`, que contém a âncora de confiança.
As chaves privadas foram mantidas no ZIP apenas para a reprodução completa da prática.

## Evidências

- `parte1/evidencias_parte1.log`: execução original dos testes AES-GCM.
- `parte2/evidencias_parte2.log`: execução original dos três cenários de assinatura.
- `Relatorio_Tecnico_Criptografia_Aplicada.pdf`: relatório para entrega no formulário.
- `Relatorio_Tecnico_Criptografia_Aplicada.docx`: versão editável do relatório.

## Referências

- NIST. *SP 800-38D — Recommendation for Block Cipher Modes of Operation: Galois/Counter Mode (GCM) and GMAC*. 2007.
- IETF. *RFC 8017 — PKCS #1: RSA Cryptography Specifications Version 2.2*. 2016.
- IETF. *RFC 5280 — Internet X.509 Public Key Infrastructure Certificate and CRL Profile*. 2008.
- pyca/cryptography. Documentação de AESGCM: https://cryptography.io/en/latest/hazmat/primitives/aead/.
- OpenSSL. Manual de comandos: https://docs.openssl.org/3.0/man1/.
