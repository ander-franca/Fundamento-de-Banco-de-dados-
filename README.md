# Fundamento-de-Banco-de-dados-
SQL
--1) Número de vagas disponíveis no curso 'DIREITO'--
SELECT COUNT(*)
FROM curso C
WHERE C.nome LIKE 'DIREITO'

--2) Nota média dos aprovados em um 'REDE DE COMPUTADORES'--
SELECT A.p_name, (+AVG(N.valo_nota))
FROM candidato A, curso C, c_escolhe_v N
WHERE C.nome = 'REDE DE COMPUTADORES' and
	  C.area_peso = N.des_area
	  
ORDER BY A.p_name

--3) Nota de corte média dos cursos da universidade 'UNIVERSIDADE FEDERAL DO CEARÁ'--
SELECT AVG(C.nota_de_corte)
FROM universidade U, curso C
WHERE U.nome = 'UNIVERSIDADE FEDERAL DO CEARÁ' and
	  U.id_universidade = C.id_universidade


--4) Lista de candidatos a serem eleitos às vagas do curso 'REDE DE COMPUTADORES'--
SELECT A.p_name, A.u_name, N.valo_nota,
		RANK () OVER ( 
				PARTITION BY C.nome 
				ORDER BY N.valo_nota DESC
			
	) rank_no 
FROM curso C, nota N, candidato A
WHERE C.nome = 'REDE DE COMPUTADORES' and--REDES DE COMPUTADORES
	  A.cpf_candidato = N.id_candidato and
	  N.valo_nota >= C.nota_de_corte
ORDER BY N.valo_nota >= C.nota_de_corte



--5) Lista de candidatos na lista de espera do curso 'REDE DE COMPUTADORES' com nota
--acima da média dos candidatos que escolheram uma vaga nesse curso;
SELECT
FROM candidato A, nota N, curso C
WHERE C.nome = 'REDE DE COMPUTADORES' and
	  A.cpf_candidato  = N.id_candidato and
	  N.valo_nota > (SELECT AVG(N.valo_nota)
					 FROM candidato A, nota N, curso C, c_escolhe_v V
					 WHERE C.nome = 'REDE DE COMPUTADORES' and
					 	   A.cpf_candidato = V.cand_cpf and
					 	   V.id_crs = C.id_curso)


Lista de candidatos na lista de espera de um curso que eles escolheram como primeira opção;

--6) Lista das instituições participantes da seleção em ordem alfabétca--
SELECT U.nome
FROM universidade U
ORDER BY U.nome ASC

--7) Lista de cursos que a instituição 'UNIVERSIDADE DE OCARA' oferece;
SELECT C.nome
FROM universidade U, curso C
WHERE U.nome = 'UNIVERSIDADE DE OCARA' and
	  C.id_universidade = U.id_universidade

--8) Lista dos cursos de graduação oferecidos por todas as universidades por nota de corte decrescente--
SELECT C.nome, C.nota_de_corte
FROM curso C
ORDER BY C.nota_de_corte DESC

--9) Curso com maior número de concorrentes--
SELECT C.nome, COUNT(V)
FROM curso C, c_escolhe_v V
WHERE C.id_curso = V.id_crs
GROUP BY  C.nome
ORDER BY C.nome;

Quantidade de candidatos que escolheram um curso como segunda opção; e

--10) Quantidade de instituições que oferecem o mesmo curso e suas notas de corte médias--
SELECT C.nome, COUNT(*), AVG(C.nota_de_corte)
FROM universidade U, curso C
WHERE C.id_universidade = U.id_universidade
GROUP BY C.nome
