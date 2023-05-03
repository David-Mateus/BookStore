use('bookstore')
// coleções
//SELECT * FROM LIVRO;
// db.livro.find()

// ======== Consulta simples ======== //
// db.livro.find({"isbn": "000001"})

/*
Para encontrar todos os livros com
cod_estante igual a "001" e cod_biblioteca igual a "001"
*/ 
// db.livro.find({
//     $and: [
//       { cod_estante: "001" },
//       { cod_biblioteca: "001" }
//     ]
//   }).pretty();
//Para encontrar todos os livros com isbn diferente de "000001"
// db.livro.find({
//     isbn: {
//       $ne: "000001"
//     }
//   }).pretty();

/*
Para encontrar todos os 
livros com cod_estante igual a "001" e isbn maior que "000001"
*/
// db.livro.find({
//     $and: [
//       { cod_estante: "001" },
//       { isbn: { $gt: "000003" } }
//     ]
//   }).pretty();

/*
Para encontrar todos os livros com cod_biblioteca
igual a "001" e com isbn que faz parte do array do isbn[01,02,03]
*/ 

// db.livro.find({
//     $and: [
//       { cod_biblioteca: "001" },
//       { isbn:{$in: ["000003", "000001", "000002"]} }
//     ]
//   }).pretty();



// db.bibliotecario.aggregate([
//   {$group: {_id: {cod_bibliotecario: "$cod_bibliotecario",nome: "$nome"}}}
// ])

//findOne() -> retorna o primeiro resultado
//find -> retorna todos os documentos
// simples query

//Irá retorna os dados que atendem
// ser menor  8 e seja diferente do autor 
// db.livro.find({
//   $or:[
    
//     {isbn:{$lt: "000008"}}
//   ],
//   $and:[
//     {autor:{$ne: "João Cabral de Melo Neto"}}
//   ]
// }).pretty()


// db.bibliotecario.find({
//   $and: [
//     { cod_biblioteca: { $ne: "002" } },
//     { $or: [
//         { "biblioteca.end_numero": { $lt: 20 } },
//         { "biblioteca.end_numero": { $gte: 30 } }
//       ]
//     }
//   ]
// }).pretty()

/*retorna todas as estantes que  são diferente da 
 sessao 5 e que retornam cod_biblioteca igual a
002 ou 003*/
// db.estante.find({
//   $or:[
//     {cod_biblioteca: {$eq: "003"}},
//     {cod_biblioteca: {$eq: "002"}}
//   ],
//   $and:[
//     {sessao:{$ne:"SESSAO 5"}}
//   ]
// }).pretty()
/*
Retorna todos cpf  igual que 100 e os alugueis que
que foram feitos na biblioteca 001 ou 003
*/
// db.aluga.find(
//   {
//     $and:[
//         {cpf:{$eq:'100'}},
//     ],
//     $or:[
//       {cod_biblioteca:{$eq:"003"}},
//       {cod_biblioteca:{$eq:"001"}}
//     ]
//   }
// ).pretty();

// ==============================//=======================//
/**
 * retorna todos os alugueis que foram feitos na biblioteca 001 ou 003
 * e que tem cpf maiores ou iguais a 10
 */
// db.aluga.find({
//   $and: [
//     { cpf: { $gte: "10" } },
//     { cod_biblioteca: { $in: ["001", "003"] } }
//   ]
// }).pretty();

/**
 * Buscar todos os bibliotecários que trabalham em bibliotecas
 * com endereço menores que 30  e não são Fernanda Soares
 */
// db.bibliotecario.find({
//   $and: [
//     { nome: { $ne: "Fernanda Soares" } },
//     {
//       $and: [
//         {
//           $expr: {
//             $lte: ["$cod_biblioteca.end_numero", "30"]
//           }
//         }
//       ]
//     }
//   ]
// }).pretty();
/**
 * Retorna todos os livros que tem autor igual a J. R. R. Tolkien
 * e que não são da biblioteca 001
 */
// db.livro.find({
//   $and:[
//     {autor:{$eq:"J. R. R. Tolkien"}},
//     {
//       $or:[
//         {
//           $expr:{
//             $ne:["$cod_biblioteca","001"]
//           }
//         }
//       ]
//     }
//   ]
// }).pretty();
/**
 * retorna todos os alugueis que foram feitos na biblioteca 003
 * e que tem uma data maior 15 e igual a '2023-03-23'
 */
// db.aluga.find({
//   $and:[
//     {"data_cria":{$gte:ISODate("2023-03-15")}},
//     {"data_cria":{$eq:ISODate("2023-03-23")}}
//   ],
//   $or:[
//     {
//       $expr:{
//         $eq:["$cod_biblioteca","003"]
//       }
//     }
//   ]
// }).pretty();

// db.biblioteca.aggregate([
//   {
//     $lookup: {
//       from: "bibliotecario",
//       localField: "end_numero",
//       foreignField: "nome",
//       as: result
//     }
//   }
// ])