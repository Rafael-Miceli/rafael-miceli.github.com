---
author: Rafael Miceli
comments: true
layout: default 
category : dotnet
title: "Trabalhando com LINQ Intersect" 
date: 2017-04-18 23:50:00
---

No nosso dia a dia geralmente vamos precisar manipular muitas listas e os extensions methods do Linq __Intersect__ e __Except__ nos dão um excelente adianto quando precisamos comparar duas listas diferentes!

O __Intersect__, ele é um extension Method que usamos em qualquer derivado do __IEnumerable__. O __Intersect__ compara os valores de outro derivado do __IEnumerable__ com quais valores se repetem entre as listas e retorna uma nova lista com os valores que se repetiam de ambas.

O __Except__ é similar ao __Intersect__ com a diferença que ao invês de valores que contêm em ambas as listas ele retorna os valores que __não__ se repetem nas listas.

A biblioteca da [MSDN](link) tem exemplos e uma descrição. Mas então para que esse post? Porque senti falta de atentar a alguns detalhes.

### Detalhes importantes!

__Intersect__ retorna apenas 1 item que se repete entre as listas. Se você possui mais de uma repetição em uma lista como em um caso:

`var lista1 = {1,2,3,4}`
`var lista2 = {1,1,1,5}`

O resultado do __Intersect__ vai ser apenas `listaresult = {1}`. Pois mesmo o `1` repetindo diversas vezes em uma lista ele traz apenas um. E isso apesar de ser fácil de observar neste exemplo pode não ser trivial em outros como o abaixo:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;


namespace ConsoleApplication
{
    public class Program
    {
        public static void Main(string[] args)
        {
            List<Aluno> alunosExistentes = BuscarAlunosNaBase();
            List<Aluno> alunosImportando = BuscarAlunosDoExcel();

            IEnumerable<Aluno> alunosRepetidos = alunosImportando.Intersect(alunosExistentes);

            foreach (var alunoRepetido in alunosRepetidos)
                Console.WriteLine(alunoRepetido.Nome);
        }

        private static List<Aluno> BuscarAlunosNaBase()
        {
            return new List<Aluno> {
                new Aluno {
                    Nome = "Rafael",
                    Idade = 18,
                    Genero = "Masculino",
                    TipoCabelo = "Cacheado"           
                },
                new Aluno {
                    Nome = "Priscilla",
                    Idade = 17,
                    Genero = "Feminino",
                    TipoCabelo = "Ondulado"           
                },
                new Aluno {
                    Nome = "Marcos",
                    Idade = 17,
                    Genero = "Masculino",
                    TipoCabelo = "Cacheado"           
                },
                new Aluno {
                    Nome = "Joao",
                    Idade = 17,
                    Genero = "Masculino",
                    TipoCabelo = "Liso"           
                },
                new Aluno {
                    Nome = "Alice",
                    Idade = 21,
                    Genero = "Feminino",
                    TipoCabelo = "Liso"           
                },
                new Aluno {
                    Nome = "Ariel",
                    Idade = 16,
                    Genero = "Feminino",
                    TipoCabelo = "Ondulado"           
                }
            };
        }

        private static List<Aluno> BuscarAlunosDoExcel()
        {
            return new List<Aluno> {
                new Aluno {
                    Nome = "Rafael",
                    Idade = 18,
                    Genero = "Masculino",
                    TipoCabelo = "Cacheado"           
                },
                new Aluno {
                     Nome = "Ariel",
                    Idade = 16,
                    Genero = "Feminino",
                    TipoCabelo = "Liso"           
                },
                new Aluno {
                    Nome = "Ariel",
                    Idade = 16,
                    Genero = "Feminino",
                    TipoCabelo = "Ondulado"           
                }
            };            
        }
    }

    public class Aluno
    {
        public string Nome { get; set; }
        public int Idade { get; set; }
        public string Genero { get; set; }
        public string TipoCabelo { get; set; }

        public override bool Equals (object obj)
        {            
            if (obj == null || GetType() != obj.GetType())
            {
                return false;
            }

            var y = (Aluno)obj;
            
            if (Object.ReferenceEquals(this, y)) return true;

            return  Nome.Equals(y.Nome) && Idade.Equals(y.Idade) && Genero.Equals(y.Genero);            
        }
        
        public override int GetHashCode()
        {            
            int hashAlunoNome = Nome == null ? 0 : Nome.GetHashCode();

            int hashAlunoIdade = Idade.GetHashCode();

            return hashAlunoNome ^ hashAlunoIdade;
        }
    }
}
```

No código acima, nossa lista `alunosRepetidos` deveria gerar 3 alunos, mas gera apenas 2, mesmo um deles não sendo __"idênticos"__ a nossos olhos comuns, mas nosso código considera idêntico em seu `Equals`.

O recado é, atentem ao __Intersect__ quando tratando objetos complexos com situações como essas. Pode lhe tomar um tempo...