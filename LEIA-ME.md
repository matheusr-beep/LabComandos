# LabComandos v0.1 — protótipo para testes

**Não é um instalador pronto.** Este ZIP contém o código-fonte e a automação de compilação no GitHub Actions. Não foi compilado nem testado em Windows neste ambiente.

## Como gerar os EXEs
1. Crie um repositório **privado** no GitHub.
2. Envie o conteúdo deste ZIP para a raiz do repositório, incluindo a pasta `.github/workflows`.
3. Em **Actions > Compilar Windows > Run workflow**, execute a compilação.
4. Em **Artifacts**, baixe `LabComandos-Windows` e extraia os dois executáveis.
5. Se a compilação falhar, envie o log do Actions para corrigirmos.

## Preparar dois PCs de teste
1. Instale o .NET Framework 4.8 no Windows 7 SP1 e no Windows 10 se necessário. Windows 7 precisa estar atualizado para suportá-lo.
2. Crie uma chave aleatória forte (mínimo 20 caracteres; ideal 32 ou mais). **Não coloque a chave no GitHub.**
3. No Cliente, crie `%APPDATA%\LabComandos\client-key.txt` com a chave em uma linha. Inicie `LabComandosCliente.exe` na sessão do aluno. Na primeira execução válida ele registra inicialização automática para **o usuário atual**, não para todos os usuários.
4. Autorize **somente na rede privada do laboratório** a porta TCP 45980 no Firewall do Windows, restrita ao IP do Master. Não abra portas no roteador para a internet.
5. No Master, execute `LabComandosMaster.exe`, digite a mesma chave, configure as faixas `192.168.2.,192.168.3.` e clique **Localizar PCs**.
6. Teste primeiro **Abrir Chrome** e **Bloco de Notas**; só depois teste desligar e reiniciar.

## Limitações e avisos
- O Cliente deve estar aberto em uma sessão de usuário. Não é um serviço do Windows: se ninguém fizer login, não poderá receber comandos.
- Inicialização automática é feita na chave `HKCU\...\Run`; o EXE deve ficar em um local permanente e acessível. Não mova depois de executar.
- Comunicação autenticada por HMAC-SHA256, mas **não criptografada**. Use exclusivamente em LAN confiável. A chave não é transmitida pela rede; os comandos e argumentos trafegam em texto legível.
- A detecção procura endereços das faixas configuradas (separadas por vírgula) e só reconhece Clientes com a mesma chave.
- Lista de aplicativos permitidos: `notepad.exe`, `calc.exe`, `mspaint.exe`, `winword.exe`, `excel.exe`. Chrome e URLs têm botões próprios.
- Abrir Chrome pressupõe que `chrome.exe` esteja localizável pelo Windows; poderá ser necessário ajustar o caminho em algumas máquinas.
- Não inclui instalador Setup.exe, atualização remota, criptografia TLS, execução arbitrária de comandos nem configuração automática do firewall.
- Windows 7 está fora de suporte. Evite conectar essas máquinas à internet e restrinja o acesso à rede interna.
- Os retornos de abertura indicam que o Windows aceitou a solicitação, não que a janela foi renderizada corretamente.

## Redes adicionais
O Master pesquisa as duas faixas `192.168.2.1–254` e `192.168.3.1–254` por padrão. Para alcançar Clientes na outra sub-rede, é necessário que haja roteamento entre as redes e que o firewall permita a conexão TCP à porta 45980, restrita ao Master. Não é necessário abrir portas na internet.
