# WebSphere

## Aplicação básica

O diretório `basic-websphere-app` contém uma aplicação Java EE mínima empacotada como WAR e pronta para ser executada no IBM WebSphere Application Server (WAS) em um ambiente SUSE Linux Enterprise Server 11.

### Estrutura

- `pom.xml`: projeto Maven com empacotamento WAR e dependências fornecidas pelo WebSphere
- `src/main/java/com/example/websphere/HelloServlet.java`: servlet simples exposto em `/hello`
- `src/main/webapp/index.jsp`: página inicial com link para o servlet
- `src/main/webapp/WEB-INF/web.xml`: configuração básica do módulo web

## Passo a passo para SUSE Linux Enterprise Server 11

1. **Instalar pré-requisitos**
   - Certifique-se de ter o Java Development Kit (JDK) 8 instalado e configurado em `JAVA_HOME`.
   - Instale o Maven caso ainda não esteja disponível (`zypper install maven`).
   - Garanta que o IBM WebSphere Application Server (versão compatível com Java EE 7) esteja instalado e com o perfil configurado.

2. **Compilar o projeto**
   - Acesse o diretório da aplicação: `cd basic-websphere-app`.
   - Execute `mvn clean package`. O artefato `basic-websphere-app.war` será gerado em `target/`.

3. **Preparar o WebSphere**
   - Inicie o servidor: `./bin/startServer.sh server1` (ajuste o nome conforme seu perfil).
   - Verifique os logs em `profiles/<perfil>/logs/server1/SystemOut.log` para confirmar que o servidor iniciou sem erros.

4. **Implantar a aplicação**
   - Use a console administrativa (`https://<host>:9043/ibm/console`) ou a ferramenta `wsadmin.sh` para instalar o WAR.
   - Via console: `Applications > Application Types > WebSphere enterprise applications > Install`. Faça upload do arquivo `target/basic-websphere-app.war` e avance sempre aceitando os padrões sugeridos.
   - Salve as alterações e sincronize os nós, se aplicável.

5. **Validar**
   - Acesse `http://<host>:9080/basic-websphere-app/` para abrir a página JSP inicial.
   - Acesse `http://<host>:9080/basic-websphere-app/hello` para validar o servlet.

6. **Dicas de resolução de problemas**
   - Se o Maven não encontrar o JDK, exporte `JAVA_HOME=/caminho/do/java` e adicione `JAVA_HOME/bin` ao `PATH`.
   - Caso a porta 9080 esteja ocupada, verifique a configuração de porta do perfil WebSphere em `serverindex.xml`.
   - Logs detalhados ficam em `profiles/<perfil>/logs/server1/`. Use `tail -f SystemOut.log` para acompanhar em tempo real.

## Próximos passos sugeridos

- Adicionar testes de integração ou páginas extras conforme a necessidade do seu projeto.
- Automatizar a implantação com scripts `wsadmin` ou ferramentas como UrbanCode Deploy, conforme o ambiente.
