![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/azure-machine-learning.jpg)

# Lab “Explorando o Machine Learning Automatizado no Azure Machine Learning”

Compartilhando minha experiência no passo a passo do processo descrito no documento oficial da Microsoft disponível [aqui](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html).

Partimos do ponto de que a conta já está criada na Azure e sim, **precisa de um cartão de crédito**. Cartões pré-pagos e virtuais não são aceitos, como informado na página da [Microsoft](https://learn.microsoft.com/pt-br/azure/cost-management-billing/troubleshoot-billing/billing-troubleshoot-azure-payment-issues)

Também é necessário criar uma subscription, que é uma forma de agrupar os custos na conta. Imagine que você tenha dois projetos de setores (ou centros de custo) diferentes na sua empresa, e precisa separar o custo para que cada, por questões de orçamento, por exemplo. É possível usar as subscriptions para segregar os custos e tornar esse gerenciamento mais simples. Veja [este](https://www.cloud-drills.eu/which-subscription-option-should-i-choose-to-get-started-with-microsoft-azure/) artigo para se aprofundar um pouco mais neste assunto.

### Controle os custos
Para evitar surpresas, sempre verifique os custos atuais da sua subscription. Na barra do menu principal à esquerda, clique em **Cost Management + Billing**. Na página do Cost Analysis selecione a opção **Cost Analysis**. Você verá o quanto gastou. A moeda apresentada é a que esta configurada na sua conta. No exemplo, foram gastos até agora R$ 4,38 (quatro reais e trinta e oito centavos) para realizar o lab 

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/gif/CostAnalysis.gif)

## Defina o orçamento e configure um alerta
Na página do Cost Analysis, selecione a opção **Budgets** e na barra superior à direita do campo de pesquisa clique em **+ Add**. Defina um nome para, o período de vigência (por exemplo, se você quer gastar U$ 10.00 por mês, selecione a opção **Monthly** ou **Mensal** de acordo com o idioma configurado.) e clique em Next. Em **Alert Conditions** selecione a opção **Actual** para que o alerta seja referente ao valor que já foi gastou ou **Forecasted** para que o alerta seja referente ao valor previsto baseado na projeção conforme a sua utilização. Recomento criar os dois. Em **Alert recipients (email)** informe a conta de e-mail na qual você deseja receber os alertas.

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/Budget.gif)

## Acesse o Machine Learning Studio
Os recursos de Machine Learning do Azure estão disponíveis em uma outra paǵina. Para acessá-la, acesse o link [https://ml.azure.com](https://ml.azure.com). O usuário e a senha são os mesmos da sua conta na Azure. No primeiro acesso você precisará criar um novo workspace.

### Crie o Workspace

|Nome do Campo|Descrição|
|-------------|---------|
|Workspace name|Nome do Workspace. Pode ser qualquer coisa. Ex. Lab-AI900|
|Subscription|Selecione a subscription que você criou anteriormente|
|Resource group|O campo virá com um nome sugerido logo após a **(new)**. Se não gostar do nome, clique no link bem abaixo da caixa de texto, em **Create New** e informe o nome|
|Region|Selecione uma das opções disponíveis. Os valores e recursos disponíveis variam de acordo com a região. Recomendo deixar East US 2|

## Realize o Lab oficial da Microsoft

Não vamos replicar aqui todos os passos do lab por dois motivos: Primeiro porque já está descrito no documento do lab disponível neste [link](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html). Segundo porque os serviços de nuvem são atualizados e essas mudanças podem refletir no passo a passo do lab.

Vou compartilhar aqui como contornei uma dificuldade na execução e o meu resultado.

### Falha na execução do Lab

Na execução foram apresentados erros, o que pode ser frustante. Atente-se às mensagens para conseguir contorná-los. Esse é o primeiro passo para buscar ajuda. Você já sabe, mas não custa lembrar: **SEMPRE LEIA A MENSAGEM NO LOG**.

![Erros na execução dos Jobs](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/Errors_Lab1.png)

O primeiro erro apresentado foi "Error: Shared compute is not applicable to serverless job as the following requirements are not met: AML direct submissions are disabled now. Pls try later."

Ponto chave para compreender o quê está acontendo:

* "Shared compute is not applicable to serverless job...". 

    - No passo 2 do lab "reate a new Automated ML job with the following settings...", na parte referente aos recursos computacionais **Compute**, observamos que o item "Select compute type" é do tipo Serverless, ou seja, não estamos criando uma máquina para rodar a tarefa de treinamento.

        ![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/Compute_Settings.png)
    
Pela mensagem do erro, dá para entender, a grosso modo, que este recurso não está aceitando submissões diretas neste momento, mas que podemos tentar novamente mais tarde. Submissões diretas ao Azure Machine Learning estão desabilitadas **agora**. Por favor tente **mais tarde**.


### Contornando a falha

Na configuraçã do Job, na opção Compute, podemos escolher entre 3 opções: **Compute cluster, Compute Instance e Serverless** (esta última é a opção selecionada no lab).

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/compute%20_options.png)

Assim, se quisermos testar o Job, podemos usar as outras opções. A mais simples e rápida é a **Compute instance**, onde basicamente vamos criar uma máquina virtual para rodar o Job. O problema é que você vai ter que pagar por isso, seja com seus créditos (a Microsoft dá U$ 200.00 em créditos na Azure para você testar os recursos nos primeiros 30 dias), seja na fatura do seu cartão.

Para criar a máquina, clique em **+ New** abaixo do campo "Select Azure ML compute instance". Na imagem abaixo tem uma instância com o nome "lab-ai-900 - Stoped" porque criei esta para rodar o job. As instâncias disponíveis sempre aparecerão neste campo, juntamente com o seu estado. Para rodar o Job, o estado da instância precisa estar como **Running**.

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/compute_crate_new1.png)


Clique em **New** e siga os passos na página. Basicamente, o que você precisa fazer é informar um nome para a sua instância e selecionar o tamanho da VM. Aqui eu escolhi o mesmo sugerido pelo lab porque a diferença para a instância menor foi de apenas U$ 0.01 (um centavo de dólar) por hora. Uma máquina menor vai levar mais tempo para realizar o mesmo trabalho, então essa diferença, embora pequena, não seria necessáriamente uma vantagem.

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/compute_crate_new2.png)

Na página seguinte vem o ponto que devemos nos atentar. Não esqueça de ativar a opção **Auto shut down** e definir o tempo inatividade para que a máquina seja desligada automaticamente, evitando custos!

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/compute_crate_new3.png)

O tempo mínimo é de 15 munitos, mas na opção **Customized schedules** você pode customizar quando quer que a intância seja iniciada e desligada.

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/compute_crate_new4.png)

Agora basta selecionar a instância criada e seguir com a configuração e execução do seu Job de treinamento de Machine Learning!


## Resultados do Lab

O Objetivo do lab não é analisar os resultados, mas sim entender como funciona o serviço Azure Machine Learning.

O modelo apresenta algumas métricas da predição, neste caso, de aluguel de bicicletas. 

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/job_results_metrics.png)

Após o deploy do modelo, podemos realizar o teste conforme orientações do roteiro do lab. Obtivemos a predição de 327.1418228714915 aluguéis de bicicleta

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/job_results_metrics2.png)

Conforme novos dados são gerados com o passar do tempo, o modelo pode ser processado novamente, fazendo o re-treino, ou aplicadas técnicas para que esse processo seja real-time.

Fatos excepcionais também devem ser considerados na preparação do dataset. Por exemplo, na pandemia os aluguéis de bicicleta provavelmente tiveram uma queda drástica, mas esse evento representa algo extraordinário, que deveria ser desconsiderado em uma cenário típico.

## Limpando o ambiente.

É importante excluir todos os recursos criados e que são passíveis de cobrança. No roteiro do lab temos as orientações necessários para a limpeza, mas caso você tenha criado uma instância de máquina virtual para rodar o modelo, lembre-se de excluí-la.

A instância não aparece na sua conta padrão do Azure, apenas no Azure Machine Learning Studio. Selecione a instância e clique em **Delete**.

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/compute_resource.png)

Retorne ao Cost Analysis e verifique o custo atual e a previsão. **Tenha sempre o Budget Alert configurado!**

![](https://github.com/jeffersonmoreira-ti/azureml/blob/main/assets/images/jpeg/cost_analysis_final.png)

## Conclusão

Vimos como funciona a funcionalidade de Machine Learning na plataforma Azure com o Azure Machine Learning Studio, que fornece recursos para que você possa aplicar Machine Learning sem se preocupar com infraestrutura e desenvolvimento, uma vez que a plataforma abstrai tudo isso para você. O que você precisa fazer é trazer os seus dados para que os modelos sejam treinados e validar os resultados. Vimos também como interpretar um erro e como contorná-lo para conseguirmos executar o lab com sucesso.

Apresentamos os resultados obtidos no lab e como a revisão humana é importante.

Por fim, reforçamos a necessidade de limpeza do ambiente, o acompanhamento dos gastos e a configuração dos alertadas de budget.
