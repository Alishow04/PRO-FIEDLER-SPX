# PRO Fiedler SPX — V1 piloto

Sistema independente com identidade visual baseada no HUB fornecido: azul #0054A6, títulos #284480, fundo #EBEEF4, cartões brancos e navegação lateral. O HUB original e a base Firebase não foram alterados.

## Abrir

**Vercel / GitHub Pages:** publique o conteúdo desta pasta como site estático. Na Vercel, use preset Other, sem build e raiz do projeto como pasta de saída. Não há npm install obrigatório.

**Windows local:** com Python instalado, execute `iniciar.bat`; acesse http://localhost:8000. Se a primeira aba abrir antes do servidor, atualize. Alternativamente, execute `python -m http.server 8000 --bind 127.0.0.1` nesta pasta. Não abra index.html por duplo clique: módulos e leitura de PDF requerem servidor HTTP.

## Primeiro teste

1. Importar relatórios → selecione os PDFs do Benner e/ou o Excel FOLLOW-UP.
2. Confira a prévia. Soma divergente ou item ilegível bloqueia o lote para conferência.
3. Confirme. A proposta é identificada pelo número PRO; versões anteriores ficam no histórico.
4. Em Importações e revisão, abra pendências e confira classificação, moeda e possível OVC.
5. No detalhe, defina responsável, última interação, próxima ação e comentários. Confirme SPX quando a classificação estiver apenas provável.
6. Configure os filtros. Padrão: peças SPX, acima de R$ 10.000, incluídas. O limite é estritamente maior, não maior ou igual.
7. Marque propostas ou use a seleção filtrada → Relatórios → acrescente comentário → Salvar versão e visualizar → Imprimir / Salvar PDF.
8. Na impressão, selecione Salvar como PDF, A4 paisagem, gráficos de fundo ativados e cabeçalhos/rodapés do navegador desativados.
9. Exporte um backup JSON periodicamente no modo local.

## Regras implementadas

- Todas as propostas são armazenadas, independentemente do corte.
- Campos e cálculos monetários em centavos. PDF em moeda estrangeira fica identificado e fora dos totais em reais, sem conversão.
- Data do PDF = data/hora do status. Excel preserva a data legada como emissão informada, sem inventar histórico anterior.
- Arquivo antigo não substitui uma versão com data posterior. Mesmo número PRO não cria outra oportunidade.
- Classificação por item: 13 códigos coringa fornecidos, regras de descrição e correções manuais.
- Códigos próprios desconhecidos usam sugestões conservadoras. SPX provável requer confirmação; a alternativa é o valor total da PRO, explicitamente identificado.
- Com todos os itens identificados, o corte usa apenas o subtotal SPX do tipo escolhido. Não SPX é excluído desse subtotal.
- TAG preservada como descrição; código pode identificar a categoria mesmo sem descrição do produto.
- Valores unitários positivos até R$ 0,10 e referências OVC geram sinalização, nunca exclusão automática por preço. A exclusão operacional é manual e exige motivo.
- Importação preserva comentários, agendamentos e exclusões. Correção de item é invalidada se a descrição da linha mudar.
- Regras reutilizáveis são por código + descrição, não apenas por código coringa.
- Cobranças e motivos do Benner são preservados como texto, não alteram automaticamente o status.
- ECS explícitas extraídas das observações; vínculo adicional manual. Consulta à coleção ecsRecords disponível após login.
- Relatório congela seleção, valores, classificações, comentários, data de referência dos indicadores e gráficos. Importações futuras não mudam versões já salvas.
- Gráficos: valor por status, principais clientes e vendedores por valor em aberto, próximas ações. Sem alegar que movimentações são novas emissões.

## Dados locais e compartilhamento

**Ao abrir, a V1 usa modo piloto local.** Dados ficam em localStorage deste navegador/origem e não são compartilhados entre computadores. O seletor de responsável local é uma indicação, não autenticação. Nenhum arquivo comercial vem pré-carregado no projeto distribuído.

A opção Entrar com conta Fiedler usa Firebase Authentication do projeto central-spx-fiedler, como o ECS. Antes de usar:

1. Acrescente os blocos de `firestore-pro.rules.txt` dentro das regras existentes. Não substitua as regras do HUB/ECS. As novas coleções são proRecords, proReports e proWorkspace.
2. Cadastre o domínio publicado em Authentication → domínios autorizados, quando necessário.
3. Entre com a conta já existente de Alison, Cauã ou Murilo.
4. A base compartilhada começa independente do piloto. Importe os arquivos nela; não há envio automático dos dados locais.
5. Recarregue e entre novamente para buscar mudanças de outros usuários. Esta V1 não tem sincronização em tempo real. Uma revisão global por transação impede sobrescrita silenciosa concorrente: se outro usuário tiver salvo, a gravação é rejeitada e será necessário recarregar.

O conector Firebase e o trecho de regras estão preparados, mas **não foram testados com credenciais reais nem publicados nesta entrega**. Os testes funcionais foram realizados no modo local. Primeira operação compartilhada requer validação com as contas da equipe. Relatórios muito grandes podem ultrapassar limites do Firestore; reduza a seleção se houver erro. Lotes compartilhados limitados a 400 documentos alterados por operação. Histórico e regras acumulados podem exigir reorganização do banco em uma versão futura.

## Ligar ao HUB

Em Configurações, informe o endereço publicado do HUB para ativar “Voltar ao HUB”. No index.html do HUB, localize MODULES, item id:'pro', e ajuste url para o endereço publicado deste sistema, status:'active', action:'Abrir propostas'. A entrega não modifica o HUB original. O código ECS é preservado, permitindo o HUB consultar proRecords em uma futura tela integrada. Autenticação usa o mesmo projeto; sessão entre domínios distintos não é automaticamente compartilhada.

## Limites conhecidos

- PDFs escaneados/imagens não são aceitos; use o relatório Benner com texto selecionável. Mudanças de layout precisam de revalidação do leitor.
- PDF não traz cadastro completo de produtos ou clientes; categorias desconhecidas exigem conferência. Apelidos da planilha e razões sociais do PDF não são fundidos automaticamente nos gráficos.
- Em conflito de conteúdo com exatamente o mesmo horário, o último PDF importado prevalece; todas as versões diferentes ficam no histórico.
- A V1 não interpreta automaticamente a semântica de cancelamentos/substituições nem produz resumos com IA.
- Relatório inclui comentários das propostas selecionadas. Revise conteúdo antes de compartilhar.
- Em modo local, armazenamento tem limite do navegador. Falha de gravação é sinalizada; exporte backup e use a base compartilhada para operação continuada.
- Exportação PDF é pelo diálogo de impressão do navegador, não download automático.

## Validação realizada

- Três PDFs: 40 registros, 39 PROs únicas; totais dos itens reconciliados em todos.
- PRO-428735: duas versões, valor atual 26.163,75; ordem inversa e reimportação mantêm estado mais recente e comentários.
- Excel: 61 linhas, 59 PROs únicas (424469 e 425820 repetidas); duas PROs coincidem com os PDFs. Base conjunta = 96 PROs únicas.
- Migração de comentários de OV e datas; classificação coringa; exclusão e moeda estrangeira.
- Navegador Chromium: importação PDF e Excel, edição, reimportação, filtros, relatório congelado e impressão PDF; nenhuma exceção JavaScript no fluxo validado.
- Revisão visual desktop, celular e PDF A4 paisagem.
- `node tests/core.test.mjs` executa os testes unitários das regras principais.

## Dependências

PDF.js (Mozilla, Apache-2.0) e SheetJS CE 0.20.3 (Apache-2.0) incluídos em vendor para leitura local. Firebase Web SDK é carregado do CDN oficial somente ao abrir conexão. Logotipo e paleta vieram das referências fornecidas. Aplicação HTML/CSS/JS, sem etapa de build.
