# Relatório Prático: Testes End-to-End com Cypress

## Participantes

- Fernando Gazzana
- Vinicius Silva

## Objetivo

O trabalho teve como objetivo a prática de testes End-to-End (E2E) utilizando o **Cypress** no projeto `demo-cypress`, implementando um fluxo de teste de compra na aplicação `micro-livraria`.

## Metodologia

Devido a restrições de ambiente, a execução via contêineres (Docker/Podman) foi substituída pela **instalação e execução local** das dependências (`npm install`) e da aplicação (`npm start`). Os testes foram executados com sucesso em modo _headless_ (linha de comando).

## Resultados da Execução

Todos os testes foram executados com sucesso, validando tanto o teste de exemplo (`spec1.js`) quanto o fluxo de compra completo implementado em `spec2.js`.

| Arquivo de Teste | Testes Passados | Status | Detalhes |
| :-- | :-- | :-- | :-- |
| `spec1.js` | 1/1 | **Sucesso** | Teste de asserção inicial. |
| `spec2.js` | 4/4 | **Sucesso** | Fluxo completo: Visita à página, Verificação de item, Cálculo de Frete e Compra de Livro. |
| **Total** | **5/5** | **Sucesso** | Todos os testes concluídos com êxito. |

## Código do Teste (`spec2.js`)

Abaixo está o código implementado que cumpriu as Tarefas #1 e #2 do roteiro.

```javascript
describe('Teste End-to-End', () => {
  it('Teste 1: Visita Página', () => {
    // abre o site
    cy.visit('http://localhost:5000/');
  });
  it('Teste 2: Verifica item na página', () => {
    // Verifica se existe o livro desejado
    cy.get('[data-id=3]').should('contain.text', 'Design Patterns');
  });
  it('Teste 3: Calcula Frete', () => {
    // Calcula o frete
    cy.get('[data-id=3]').within(() => {
      cy.get('input').type('10000-000');
      cy.contains('Calcular Frete').click();
      cy.wait(2000);
    });
    cy.get('.swal-text').contains('O frete é: R$');

    // Fecha o pop-up com o preço do frete
    cy.get('.swal-button').click();
  });
  it('Teste 4: Compra Livro', () => {
    // Clica no botão Comprar
    cy.get('[data-id=3]').within(() => {
      cy.contains('Comprar').click();
    });
    cy.wait(2000);
    // Verifica a mensagem de sucesso
    cy.get('.swal-text').contains('Sua compra foi realizada com sucesso');
    // Fecha o pop-up
    cy.get('.swal-button').click();
  });
});
```
