<Devolução de Equipamento>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cadastro de Devolução de Equipamento</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h1 {
            text-align: center;
        }
        .form-container {
            width: 50%;
            margin: 0 auto;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 8px;
            background-color: #f9f9f9;
        }
        .form-container label {
            font-size: 14px;
            margin-bottom: 6px;
        }
        .form-container input,
        .form-container select {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .form-container button {
            width: 100%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            font-size: 16px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .form-container button:hover {
            background-color: #45a049;
        }
        .termo-container {
            display: none;
            margin-top: 20px;
            text-align: center;
        }
        .termo-container pre {
            font-family: "Courier New", Courier, monospace;
            white-space: pre-wrap;
            word-wrap: break-word;
            text-align: left;
            margin: 0 auto;
            width: 80%;
        }
        .termo-container button {
            margin-top: 20px;
        }
        .cursos-cadastrados {
            margin-top: 30px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
            text-align: left;
        }
        th, td {
            padding: 8px;
        }
        th {
            background-color: #f2f2f2;
        }
        .btn-editar,
        .btn-excluir {
            padding: 5px 10px;
            border: none;
            cursor: pointer;
        }
        .btn-editar {
            background-color: #4CAF50;
            color: white;
        }
        .btn-editar:hover {
            background-color: #45a049;
        }
        .btn-excluir {
            background-color: #f44336;
            color: white;
        }
        .btn-excluir:hover {
            background-color: #e41c1c;
        }
        .filtro-container {
            margin: 20px 0;
        }
        .filtro-container select {
            width: 200px;
            margin-right: 10px;
        }
        @media print {
            body {
                margin: 0;
                padding: 0;
                text-align: center;
            }
            .termo-container {
                display: block;
                margin-top: 80px;
                text-align: center;
                width: 100%;
            }
            .termo-container pre {
                font-size: 16px;
                line-height: 1.5;
                margin: 0 auto;
                width: 80%;
                text-align: left;
                display: block;
            }
            .termo-container button {
                display: none;
            }
            .form-container, .cursos-cadastrados, .filtro-container {
                display: none;
            }
            h1 {
                display: none;
            }
        }
    </style>
</head>
<body>

    <h1>Cadastro de Devolução de Equipamento</h1>

    <div class="form-container">
        <form id="formCadastro" onsubmit="gerarTermo(event)">
            <label for="nome">Nome Completo:</label>
            <input type="text" id="nome" name="nome" required>

            <label for="cpf">CPF:</label>
            <input type="text" id="cpf" name="cpf" required>

            <label for="matricula">Número de Matrícula:</label>
            <input type="text" id="matricula" name="matricula" required>

            <label for="serie">Série:</label>
            <input type="text" id="serie" name="serie" required>

            <label for="turma">Turma:</label>
            <input type="text" id="turma" name="turma" required>

            <label for="turno">Turno:</label>
            <input type="text" id="turno" name="turno" required>

            <label for="curso">Curso:</label>
            <select id="curso" name="curso" required>
                <option value="">Selecione o curso</option>
                <option value="MédioTec">MédioTec</option>
            </select>

            <label for="equipamento">Equipamento (Chromebook):</label>
            <input type="text" id="equipamento" name="equipamento" placeholder="Ex: Chromebook Samsung KT3BR" required>

            <label for="numeroSerie">Número de Série:</label>
            <input type="text" id="numeroSerie" name="numeroSerie" required>

            <button type="submit" id="submitButton">Gerar Termo de Devolução</button>
        </form>
    </div>

    <div class="termo-container" id="termoContainer">
        <h2>Termo de Devolução de Equipamento Eletrônico</h2>
        <pre id="termoTexto"></pre>
        <button onclick="imprimirTermo()">Imprimir Termo</button>
    </div>

    <div class="filtro-container">
        <button onclick="mostrarLista()">Lista</button>
        <label for="filtroTurma">Filtrar por Turma:</label>
        <select id="filtroTurma" onchange="filtrarLista()">
            <option value="">Todas as turmas</option>
        </select>

        <label for="filtroTurno">Filtrar por Turno:</label>
        <select id="filtroTurno" onchange="filtrarLista()">
            <option value="">Todos os turnos</option>
        </select>
    </div>

    <div class="cursos-cadastrados">
        <h2>Alunos Cadastrados</h2>
        <table id="tabelaAlunos">
            <thead>
                <tr>
                    <th>Nome</th>
                    <th>Curso</th>
                    <th>Matrícula</th>
                    <th>Turma</th>
                    <th>Turno</th>
                    <th>Ações</th>
                </tr>
            </thead>
            <tbody>
                <!-- Alunos cadastrados aparecerão aqui -->
            </tbody>
        </table>
    </div>

    <script>
        let alunoEditandoIndex = null;

        function gerarTermo(event) {
            event.preventDefault();

            const nome = document.getElementById("nome").value;
            const cpf = document.getElementById("cpf").value;
            const matricula = document.getElementById("matricula").value;
            const serie = document.getElementById("serie").value;
            const turma = document.getElementById("turma").value;
            const turno = document.getElementById("turno").value;
            const curso = document.getElementById("curso").value;
            const equipamento = document.getElementById("equipamento").value;
            const numeroSerie = document.getElementById("numeroSerie").value;

            const aluno = {
                nome,
                cpf,
                matricula,
                serie,
                turma,
                turno,
                curso,
                equipamento,
                numeroSerie
            };

            let alunos = JSON.parse(localStorage.getItem('alunos')) || [];

            if (alunoEditandoIndex !== null) {
                alunos[alunoEditandoIndex] = aluno;
                alunoEditandoIndex = null;
                document.getElementById("submitButton").textContent = "Gerar Termo de Devolução";
            } else {
                alunos.push(aluno);
            }

            localStorage.setItem('alunos', JSON.stringify(alunos));
            listarAlunos();

            const termo = `
Eu, ${nome} portador do CPF ${cpf},Aluno do curso ${curso}, de Matrícula ${matricula} na Instituição Senac Paulista, da Turma/Turno ${turma}/${turno}, no ano letivo de 2024. Devolvo o equipamento à Instituição nas condições descritas abaixo, no mesmo estado em que o recebi no início do ano letivo, em plenas condições de funcionamento.

• Equipamento: ${equipamento} com seu Carregador
• Marca/Modelo: Samsung / KT3BR
• Número de Série: ${numeroSerie}

Além disso, confirmo que o equipamento está acompanhado do seu carregador, e que nenhum dano foi causado durante o período de uso. Em caso de danos ou extravios, comprometo-me em arcar com os custos de reparo ou reposição do equipamento, conforme estabelecido em contrato no ato do início do ano letivo.Assim, a presente devolução é feita com a anuência do Senac Paulista.

             ASSINATURA DO ALUNO
___________________________________________________

REPRESENTANTE DA INSTITUIÇÃO
Valdir Rodrigues da Silva Junior
Monitor de TI

Paulista, ______ de ____________________ de 2025.
            `;

            document.getElementById("termoTexto").textContent = termo;
            document.getElementById("termoContainer").style.display = 'block';
        }

        function listarAlunos() {
            const alunos = JSON.parse(localStorage.getItem('alunos')) || [];
            const tabelaAlunos = document.getElementById("tabelaAlunos").getElementsByTagName("tbody")[0];
            tabelaAlunos.innerHTML = "";

            alunos.forEach((aluno, index) => {
                const row = tabelaAlunos.insertRow();
                row.innerHTML = `
                    <td>${aluno.nome}</td>
                    <td>${aluno.curso}</td>
                    <td>${aluno.matricula}</td>
                    <td>${aluno.turma}</td>
                    <td>${aluno.turno}</td>
                    <td>
                        <button class="btn-editar" onclick="editarAluno(${index})">Editar</button>
                        <button class="btn-excluir" onclick="excluirAluno(${index})">Excluir</button>
                    </td>
                `;
            });

            preencherFiltros(alunos);
        }

        function preencherFiltros(alunos) {
            const filtroTurma = document.getElementById("filtroTurma");
            const filtroTurno = document.getElementById("filtroTurno");

            filtroTurma.innerHTML = "<option value=''>Todas as turmas</option>";
            filtroTurno.innerHTML = "<option value=''>Todos os turnos</option>";

            const turmas = [...new Set(alunos.map(a => a.turma))];
            const turnos = [...new Set(alunos.map(a => a.turno))];

            turmas.forEach(turma => {
                const option = document.createElement("option");
                option.value = turma;
                option.textContent = turma;
                filtroTurma.appendChild(option);
            });

            turnos.forEach(turno => {
                const option = document.createElement("option");
                option.value = turno;
                option.textContent = turno;
                filtroTurno.appendChild(option);
            });
        }

        function filtrarLista() {
            const filtroTurma = document.getElementById("filtroTurma").value;
            const filtroTurno = document.getElementById("filtroTurno").value;
            const alunos = JSON.parse(localStorage.getItem('alunos')) || [];

            const alunosFiltrados = alunos.filter(aluno => {
                const turmaMatch = filtroTurma ? aluno.turma === filtroTurma : true;
                const turnoMatch = filtroTurno ? aluno.turno === filtroTurno : true;
                return turmaMatch && turnoMatch;
            });

            const tabelaAlunos = document.getElementById("tabelaAlunos").getElementsByTagName("tbody")[0];
            tabelaAlunos.innerHTML = "";

            alunosFiltrados.forEach((aluno, index) => {
                const row = tabelaAlunos.insertRow();
                row.innerHTML = `
                    <td>${aluno.nome}</td>
                    <td>${aluno.curso}</td>
                    <td>${aluno.matricula}</td>
                    <td>${aluno.turma}</td>
                    <td>${aluno.turno}</td>
                    <td>
                        <button class="btn-editar" onclick="editarAluno(${index})">Editar</button>
                        <button class="btn-excluir" onclick="excluirAluno(${index})">Excluir</button>
                    </td>
                `;
            });
        }

        function editarAluno(index) {
            const alunos = JSON.parse(localStorage.getItem('alunos')) || [];
            const aluno = alunos[index];
            document.getElementById("nome").value = aluno.nome;
            document.getElementById("cpf").value = aluno.cpf;
            document.getElementById("matricula").value = aluno.matricula;
            document.getElementById("serie").value = aluno.serie;
            document.getElementById("turma").value = aluno.turma;
            document.getElementById("turno").value = aluno.turno;
            document.getElementById("curso").value = aluno.curso;
            document.getElementById("equipamento").value = aluno.equipamento;
            document.getElementById("numeroSerie").value = aluno.numeroSerie;

            alunoEditandoIndex = index;
            document.getElementById("submitButton").textContent = "Editar Devolução";
        }

        function excluirAluno(index) {
            let alunos = JSON.parse(localStorage.getItem('alunos')) || [];
            alunos.splice(index, 1);
            localStorage.setItem('alunos', JSON.stringify(alunos));
            listarAlunos();
        }

        function imprimirTermo() {
            window.print();
        }

        window.onload = listarAlunos;
    </script>

</body>
</html>
