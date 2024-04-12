# Javascript

## Como rodar os testes

No terminal, execute os comandos:

```bash
cd javascript
yarn
yarn test
```

Ou usando o NPM:

```bash
cd javascript
npm install
npm test
```

## Dersenvolvimento

Comecei determinando o limite máximo de funcionários que podem estar ausentes, baseado na metade do número total de funcionários de CS. Isso garante que sempre haja equipe disponível para atender aos clientes.

```js customer-success-balancing.js"
const vacationLimit = Math.floor(customerSuccess.length / 2);
```

Em seguida, verifiquei se o número de funcionários ausentes excede esse limite. Se sim, retorno uma mensagem indicando que é necessário ajustar o cronograma para garantir a disponibilidade adequada da equipe.

```js customer-success-balancing.js"
if (customerSuccessAway.length > vacationLimit) {
  return `Sorry, the number of unavailable employees exceeds the limit (${vacationLimit}). Please adjust the schedule.`;
}
```

Caso contrário, eu continuei filtrando os funcionários disponíveis, removendo aqueles que estão marcados como ausentes.

```js customer-success-balancing.js"
const availableEmployees = customerSuccess.filter((employee) => {
  if (!customerSuccessAway.find((employeeAway) => employeeAway === employee.id))
    return true;
});
```

Ordenei esses funcionários com base em suas habilidades, do menos habilidoso para o mais habilidoso.

```js customer-success-balancing.js"
const availableEmployeesAscending = availableEmployees
  .slice()
  .sort((a, b) => a.score - b.score);
```

Em seguida, criei uma cópia dos clientes para garantir que não alteraria os dados originais.

```js customer-success-balancing.js"
const customersCopy = [...customers];
```

Ao percorrer os funcionários disponíveis, associei clientes a eles com base em suas habilidades compatíveis.

```js customer-success-balancing.js"
availableEmployeesAscending.forEach((employee) => {
  employee.clients = [];
  for (let j = 0; j < customersCopy.length; j++) {
    if (employee.score >= customersCopy[j].score) {
      employee.clients.push(customersCopy[j].id);
      customersCopy[j].score = 100000;
    }
  }
});
```

Por fim, encontrei o funcionário que atendia ao maior número de clientes e retornei o ID desse funcionário. Se houvesse um empate, retornava 0.

```js customer-success-balancing.js"
csWithMostClients = availableEmployeesAscending.reduce((previous, current) => {
  if (previous.clients.length > current.clients.length) {
    return previous;
  } else if (previous.clients.length === current.clients.length) {
    return { id: 0, clients: current.clients };
  } else {
    return current;
  }
});
return csWithMostClients.id;
```

## Conclusão

Essa abordagem me permitiu resolver o problema de distribuição de clientes entre os funcionários de Customer Success de forma eficiente e garantir que os clientes fossem atendidos da melhor maneira possível, levando em consideração as habilidades e disponibilidade dos funcionários.
