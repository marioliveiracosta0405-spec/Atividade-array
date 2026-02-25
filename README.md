<!DOCTYPE html>
<html lang="pt-br">

<head>
    <meta charset="UTF-8" />
    <title>Atividade 2 Web</title>
</head>

<body>

    <h1>Cadastro de Produtos (Array de Objetos)</h1>

    <div>

        <span>Produto: </span><input type="text" id="produto" placeholder="Digite um Produto">
        <span>Preço (R$): </span><input type="text" id="preco" placeholder="Digite o preço">
        <span>Quantidade: </span><input type="text" id="quantidade" placeholder="Digite a quantidade">
        <button onclick="produto.adicionar()">Adicionar
            produto</button><!--o onclick="item.adicionar()" serve para que quando clicarem no botão seja executado a função-->
        <button onclick="produto.listaTabela()">Listar produtos</button>
        <button onclick="produto.resumo()">Resumo</button>
        <button onclick="produto.limpar()">Limpar tudo</button>
    </div>

    <div>
        <h2>Remover por Índice:</h2>
        <input type="text" id="indice" placeholder="Digite o índice">
        <button onclick="produto.removerPorIndice()">Remover</button>
    </div>

    <h3 id="fraseAd"> <span id="numArray"></span></h3>
    </div>

    <div id="Tabela" style="display: none;">
        <table border="1">
            <thead>
                <tr>
                    <th id="Id"></th>
                    <th id="produ"></th>
                    <th id="prec"></th>
                    <th id="qtde"></th>
                    <th id="sbt"></th>
                </tr>
            </thead>
            <tbody id="tbody">
                <tr>
                    <td> </td>
                    <td></td>
                </tr>
            </tbody>
        </table>
    </div>

    <p id="fraseLimp"></p>

    <div>
        <h3 id="resum"></h3>
        <p id="fraseProd"><span id="numProd"></span></p><br>
        <p id="fraseQtd"><span id="numQtd"></span></p><br>
        <p id="fraseVT"><span id="valorTotal"></span></p><br>
    </div>

    <script>
        class Produto {

            constructor() {
                this.arrayProdutos = [];//início do array
                this.id = 1;
            }

            adicionar() {
                let produto = this.pegarDados();


                if (this.validar(produto)) {
                    this.adicionar2(produto);
                    this.contIdsProd();
                }

                Tabela.style.display = 'none';

            }

            adicionar2(produto) {
                this.arrayProdutos.push(produto);//puxa os itens escritos e coloca no array
                this.id++;

            }

            validar(produto) {
                let msg = '';

                if (produto.nomeProd == '') {
                    msg += 'Digete o nome do produto \n';
                }

                if (produto.preco == '') {
                    msg += "Digete o preço \n";
                }

                if (produto.quantProd == '') {
                    msg += "Digete a quantidade de produtos \n";
                }

                if (msg != '') {
                    alert(msg);
                    return false;
                }

                return true;
            }

            pegarDados() {
                let produto = {}

                produto.id = this.id;
                produto.nomeProd = document.getElementById("produto").value;//pega o valor do id e coloca numa variável
                produto.preco = document.getElementById("preco").value;
                produto.quantProd = document.getElementById("quantidade").value;

                return produto;
            }

            listaTabela() {
                let tbody = document.getElementById('tbody')
                tbody.innerHTML = "";

                for (let i = 0; i < this.arrayProdutos.length; i++) {
                    let tr = tbody.insertRow();//cria uma nova linha pra tabela

                    let td_id = tr.insertCell();//cria uma nova coluna pra tabela
                    let td_prod = tr.insertCell();//cria uma nova coluna pra tabela
                    let td_preco = tr.insertCell();//cria uma nova coluna pra tabela
                    let td_qtd = tr.insertCell();//cria uma nova coluna pra tabela
                    let td_sb = tr.insertCell();//cria uma nova coluna pra tabela

                    td_id.innerText = i;
                    td_prod.innerText = this.arrayProdutos[i].nomeProd;
                    td_preco.innerText = parseFloat(this.arrayProdutos[i].preco).toFixed(2);
                    td_qtd.innerText = this.arrayProdutos[i].quantProd;
                    td_sb.innerText = this.arrayProdutos[i].subtotal;

                    let subtotal = (this.arrayProdutos[i].preco * this.arrayProdutos[i].quantProd);
                    td_sb.innerText = subtotal.toFixed(2);

                    document.getElementById("Id").innerText = "Índice";
                    document.getElementById("produ").innerText = "Produto";
                    document.getElementById("prec").innerText = "Preço(R$)";
                    document.getElementById("qtde").innerText = "qtd";
                    document.getElementById("sbt").innerText = "Subtotal(R$)";
                }
                Tabela.style.display = 'block';
            }

            removerPorIndice() {
                let indice = document.getElementById("indice").value;

                if (indice >= 0 && indice < this.arrayProdutos.length) {

                    this.arrayProdutos.splice(indice, 1);

                    this.contIdsProd();
                    this.listaTabela();


                }
                Tabela.style.display = 'none';
            }


            resumo() {

                const valorTotal = this.arrayProdutos.reduce((acumulador, produto) => {
                    return acumulador + (Number(produto.preco) * Number(produto.quantProd));
                }, 0);
                console.log(valorTotal);

                const totalQtd = this.arrayProdutos.reduce((acumulador, produto) => {
                    return acumulador + Number(produto.quantProd);
                }, 0);
                console.log(totalQtd);


                document.getElementById("resum").innerText = "Resumo:";
                document.getElementById("fraseProd").innerText = "Total de produtos(linhas): " + this.arrayProdutos.length;
                document.getElementById("fraseQtd").innerText = "Total de itens(somando quantidades): " + totalQtd;
                document.getElementById("fraseVT").innerText = "Valor total do estoque: " + valorTotal;

                Tabela.style.display = 'none';

            }

            limpar() {
                this.arrayProdutos = [];//limpa o array, falando que não tem nada dentro
                document.getElementById("tbody").innerHTML = ""; // limpa tabela
                document.getElementById("fraseAd").innerText = ""; // limpa contador
                this.contIdsProd();//limpa o número de arrays mostrados no contador, falando que não tem nada dentro
                document.getElementById("fraseLimp").innerText = "Todos os produtos foram removidos. "
                Tabela.style.display = 'none';
            }


            contIdsProd() {
                document.getElementById("fraseAd").innerText = "Produto adicionado. Total cadastrados: " + this.arrayProdutos.length;
              
              
            }

        } const produto = new Produto();

    </script>

</body>

</html>
