# BuscaCEPControllerTeste.apxc


@isTest

public class BuscaCEPControllerTeste {

    @isTest
    
    public static void testeConsultaCep(){
    
        /* VERIFICADO NO TRAILHEAD APEX REST CALLOUTS https://trailhead.salesforce.com/en/content/learn/modules/apex_integration_services/apex_integration_rest_callouts */
        StaticResourceCalloutMock mock = new StaticResourceCalloutMock();
        mock.setStaticResource('viaCepJson'); /* RECURSO ESTATICO COM RETORNO DO SERVIÇO CRIADO MOCK TXT.PLAIN FORMATO VIACEP*/
        mock.setStatusCode(200);
        mock.setHeader('Content-Type', 'application/json;charset=UTF-8');
        Test.setMock(HttpCalloutMock.class, mock); /* ASSOCIAR O CALLOUT AO MOCK POIS CLASSE DE TESTE NAO FAZ CALLOUT PRECISAMOS MOCKAR, NAO CONSEGUE SAIR DO SALESFORCE PARA FAZER CONSULTA */
     
        ViaCepJson result = BuscaCEPController.consultaCep('01001-000'); /* PASSAR O PARAMETRO */
       
        System.assertNotEquals(null,result);
   
    }
    /* 2 TESTES = 1 PARA TRUE E OUTRO PARA FALSE */
     @isTest
    public static void testeAtualizaEndeTrue(){
    	Account acc = new Account();
        acc.Name = 'Higgor Oliveira';
        insert acc;
        String json ='{"cep": "01001-000","logradouro": "Praça da Sé","complemento": "lado ímpar","bairro": "Sé","localidade": "São Paulo","uf": "SP","unidade": "","ibge": "3550308","gia": "1004"}';        
        
        System.assertEquals(true,BuscaCEPController.atualizaEndereco(acc.id, json));        
    }
    
    @isTest
    public static void testeAtualizaEndeFalse(){
    	Account acc = new Account();
        acc.Name = 'Higgor Oliveira';
        insert acc;
        String json ='"cep": "01001-000","logradouro": "Praça da Sé","complemento": "lado ímpar","bairro": "Sé","localidade": "São Paulo","uf": "SP","unidade": "","ibge": "3550308","gia": "1004"}';        
        /* PARA OCORRA O ERRO BASTA INVALIDAR ESTE JSON COM FORMATO INVALIDO */
        System.assertEquals(false,BuscaCEPController.atualizaEndereco(acc.id, json));        
    }
    
    
    
}
