<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Painel Técnico - Segurança do Trabalho</title>
<style>
  body { font-family: Arial, sans-serif; margin: 0; background: #f0f0f5; }
  header { background: #2c3e50; color: #fff; padding: 15px; text-align: center; }
  nav {
    background: #34495e;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
  }
  nav button {
    background: none; border: none; color: #fff; padding: 15px 20px;
    font-size: 1rem; cursor: pointer; transition: background 0.2s;
  }
  nav button:hover { background: #2c3e50; }
  section { display: none; padding: 20px; }
  section.active { display: block; }
  h2 { color: #2c3e50; border-bottom: 2px solid #2c3e50; padding-bottom: 5px; }
  button.action {
    background-color: #2c3e50; color: white; padding: 8px 14px;
    font-size: 0.9rem; border: none; border-radius: 4px; cursor: pointer;
    transition: background-color 0.3s ease;
  }
  button.action:hover { background-color: #34495e; }
  textarea, input, select {
    width: 100%; padding: 10px; margin-bottom: 15px;
    border: 1px solid #ccc; border-radius: 5px; font-size: 1rem;
  }
  .output, .bloco {
    background: #fff; padding: 15px; border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1); margin-bottom: 15px;
  }
  .bloco.fisico { border-left: 6px solid #007bff; }
  .bloco.biologico { border-left: 6px solid #28a745; }
  .bloco.quimico { border-left: 6px solid #dc3545; }
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; }
  .item { background: white; border-radius: 8px; padding: 15px; text-align: center; box-shadow: 0 2px 5px rgba(0,0,0,0.1);}
  .item img { width: 100px; height: auto; margin-bottom: 10px; }
  footer { text-align: center; font-size: 0.8rem; color: #555; padding: 10px; background: #eaeaea; margin-top: 20px; }
  small { color: #555; }
</style>
</head>
<body>

<header>
  <h1>Painel Técnico - Segurança do Trabalho</h1>
</header>

<nav>
  <button onclick="mostrar('epis')">📒 Catálogo de EPIs</button>
  <button onclick="mostrar('riscos')">🧪 Avaliação de Riscos</button>
  <button onclick="mostrar('empresas')">🏢 Frases com Empresa</button>
  <button onclick="mostrar('treinamento')">📚 Treinamento NR-06</button>
  <button onclick="mostrar('riscos_empresas')">📋 Riscos por Empresa</button>
</nav>

<main>
  <!-- EPIs -->
  <section id="epis" class="active">
    <h2>Catálogo de EPIs</h2>
    <input type="text" id="searchInput" placeholder="Buscar por nome ou CA..." onkeyup="searchItems()" />
    <select id="categoryFilter" onchange="searchItems()">
      <option value="">Todas as Categorias</option>
    </select>
    <div class="grid" id="catalog"></div>
  </section>

  <!-- Avaliação de Riscos -->
  <section id="riscos">
    <h2>Avaliação de Riscos</h2>
    <select id="filtro" onchange="filtrarRiscos()">
      <option value="todos">Todos</option>
      <option value="fisico">Físico</option>
      <option value="biologico">Biológico</option>
      <option value="quimico">Químico</option>
    </select>
    <div id="conteudo">
      <!-- Físico -->
      <div class="bloco fisico" data-risco="fisico">
        <h3>📘 FÍSICO - Parte 1</h3>
        <p>De acordo com a inspeção realizada no ambiente de trabalho e atividades executadas pelo trabalhador que desempenha este cargo, e de acordo com a NR 15 da portaria 3.214/78 do M.T.E, o mesmo está exposto a agentes ambientais nocivos a saúde ao risco Físico.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
      <div class="bloco fisico" data-risco="fisico">
        <h3>📘 FÍSICO - Parte 2</h3>
        <p>As atividades de trabalho realizadas neste LTCAT não são consideradas de Condições Especiais de Trabalho e, portanto, não são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
      <!-- Biológico -->
      <div class="bloco biologico" data-risco="biologico">
        <h3>🧬 BIOLÓGICO - Parte 1</h3>
        <p>De acordo com a inspeção realizada no ambiente de trabalho e atividades executadas pelo trabalhador que desempenha este cargo, e de acordo com a NR 15 da portaria 3.214/78 do M.T.E, o mesmo está exposto a agentes ambientais nocivos a saúde ao risco Biológico.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
      <div class="bloco biologico" data-risco="biologico">
        <h3>🧬 BIOLÓGICO - Parte 2</h3>
        <p>As atividades de trabalho realizadas neste LTCAT não são consideradas de Condições Especiais de Trabalho e, portanto, não são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
      <div class="bloco biologico" data-risco="biologico">
        <h3>🧬 BIOLÓGICO - Parte 3</h3>
        <p>As atividades de trabalho realizadas neste LTCAT são consideradas de Condições Especiais de Trabalho e, portanto, são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
      <!-- Químico -->
      <div class="bloco quimico" data-risco="quimico">
        <h3>🧪 QUÍMICO - Parte 1</h3>
        <p>De acordo com a inspeção realizada no ambiente de trabalho e atividades executadas pelo trabalhador que desempenha este cargo, e de acordo com a NR 15 da portaria 3.214/78 do M.T.E, o mesmo está exposto a agentes ambientais nocivos a saúde ao risco Químico.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
      <div class="bloco quimico" data-risco="quimico">
        <h3>🧪 QUÍMICO - Parte 2</h3>
        <p>As atividades de trabalho realizadas neste LTCAT não são consideradas de Condições Especiais de Trabalho e, portanto, não são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
      <div class="bloco quimico" data-risco="quimico">
        <h3>🧪 QUÍMICO - Parte 3</h3>
        <p>As atividades de trabalho realizadas neste LTCAT são consideradas de Condições Especiais de Trabalho e, portanto, são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button>
      </div>
    </div>
  </section>

  <!-- Frases com Empresa -->
  <section id="empresas">
    <h2>Gerador de Frases com Empresa</h2>
    <textarea id="nomes" rows="6" placeholder="Um nome por linha"></textarea>
    <input type="text" id="empresa" placeholder="Nome da empresa" />
    <input type="text" id="data" placeholder="Data" />
    <button class="action" onclick="gerarFrases()">Gerar Frases</button>
    <div class="output" id="resultado"></div>
  </section>

  <!-- Treinamento -->
  <section id="treinamento">
    <h2>Treinamento NR-06</h2>
    <div class="bloco"><h3>Parte 1</h3><p>Treinamento de NR-06</p><button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button></div>
    <div class="bloco"><h3>Parte 2</h3><p>Coordenador da empresa</p><button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button></div>
    <div class="bloco"><h3>Parte 3</h3><p>Permanente</p><button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button></div>
    <div class="bloco"><h3>Parte 4</h3><p>Ambiente laboral</p><button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button></div>
    <div class="bloco"><h3>Parte 5</h3><p>Conscientizar os colaboradores sobre o uso de EPI.</p><button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button></div>
    <div class="bloco"><h3>Parte 6</h3><p>Palestra NR-06, treinamento de EPI.</p><button class="action" onclick="copiarTexto(this.previousElementSibling.innerText)">Copiar</button></div>
  </section>

  <!-- Riscos por Empresa -->
  <section id="riscos_empresas">
    <h2>Riscos por Empresa</h2>
    <input type="text" id="buscaRiscos" placeholder="Buscar risco, empresa ou setor..." onkeyup="filtrarRiscosEmpresas()" />
    <div id="listaRiscosEmpresas"></div>
  </section>
</main>

<footer>
  Desenvolvido para fins técnicos de SST | &copy; 2025
             Desenvolvido por Vytor Suporte Técnico
</footer>

<script>
  const senhaCorreta = "SG3933"; // altere para a senha que você quiser
const secoesProtegidas = ["riscos", "treinamento", "riscos_empresas"];

function mostrar(secao) {
  // Se a seção for protegida, pede senha
  if (secoesProtegidas.includes(secao)) {
    const senhaDigitada = prompt("Digite a senha para acessar esta seção:");
    if (senhaDigitada !== senhaCorreta) {
      alert("Senha incorreta! Acesso negado.");
      return;
    }
  }
  document.querySelectorAll("section").forEach(el => el.classList.remove("active"));
  document.getElementById(secao).classList.add("active");
}

  /* -------- EPIs (do PDF) -------- */
const epis = [
  { categoria: "👢 Calçados", nome: "Botina de Elástico com Bico de PVC - Fujiwara", ca: "48.413", imagem: "https://alturaecia.com.br/wp-content/uploads/2022/03/Botina-de-Elastico-com-Bico-PVC-Usafe-Fujiwara.jpg" },
  { categoria: "👢 Calçados", nome: "Bota de Segurança Bico Composite NR10 Eletricista - Bracol", ca: "45.258", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/68/1068213_bota-seguranca-bracol-microfibra-composite-eletricista-38530_z2_638430758882818688.webp" },
  { categoria: "👢 Calçados", nome: "Sapato de Amarrar em Couro com Palmilha - Fujiwara (Branco/Preto)", ca: "41.858", imagem: "https://images.tcdn.com.br/img/img_prod/1033319/sapato_de_amarrar_fujiwara_linha_usafe_em_couro_com_palmilha_ca_41858_4098usas4600us_171_1_370dd8cc6d1594636c42615405c0579d.jpg" },
  { categoria: "👢 Calçados", nome: "Sapato Antiderrapante Impermeável - Steelflex", ca: "38.590", imagem: "https://btequipamentos.agilecdn.com.br/111067_1_1.jpg?v=220-858371529" },
  { categoria: "👢 Calçados", nome: "Bota de PVC Meio Cano (Branco/Preto) - Bracol", ca: "37.456", imagem: "https://safetytrab.com.br/wp-content/uploads/2022/08/Bota-de-PVC-Impermeavel-Cano-Curto-Preta-Bracol-CA-37456-516x516.png" },
  { categoria: "👢 Calçados", nome: "Bota de Seguranca em couro Nobuck com cadarço Dubai Eletrista bico PVC- Bracol", ca: "48.383", imagem: "https://http2.mlstatic.com/D_NQ_NP_761173-MLB89078178731_072025-O-bota-botina-de-seguranca-coturno-nobuck-marluvas-epi-com-ca.webp" },
  { categoria: "🧤 Luvas", nome: "Luva de Algodão Tricotada Mesclada - Volk", ca: "25.773", imagem: "https://i.imgur.com/xN1BFLa.png" },
  { categoria: "🧤 Luvas", nome: "Luva de Malha Neotato PU Preta - Volk", ca: "30.916", imagem: "https://i.imgur.com/c9CLP9E.png" },
  { categoria: "🧤 Luvas", nome: "Luva de Látex Multiuso para Uso Químico/Biológico (Amarela/Azul) - Danny", ca: "39.564", imagem: "https://i.imgur.com/hsiXIQa.png" },
  { categoria: "🧤 Luvas", nome: "Luva Pegasus PRO Coleta de Lixo e Serviços Gerais Bicolor - Volk", ca: "28.709", imagem: "https://i.imgur.com/BAbfvB6.png" },
  { categoria: "🧤 Luvas", nome: "Luva de Látex Neoprene - Volk", ca: "37.900", imagem: "https://i.imgur.com/y79Qsop.png" },
  { categoria: "🧤 Luvas", nome: "Luva Nitrílica Verde para Uso Químico/Biológico 35cm - Delta Plus", ca: "42.938", imagem: "https://i.imgur.com/qDHZ4XW.png" },
  { categoria: "🧤 Luvas", nome: "Luva PVC Forrada Cano Longo Palma Áspera - Danny", ca: "37.559", imagem: "https://i.imgur.com/6ML7rLO.png" },
  { categoria: "🧤 Luvas", nome: "Luva Malha de Aço - Danny", ca: "6.257", imagem: "https://i.imgur.com/nWzDsXA.png" },
  { categoria: "🧤 Luvas", nome: "Luva de Vaqueta Petroleira Crua - Protcap", ca: "15.061", imagem: "https://i.imgur.com/iX5fA0r.png" },
  { categoria: "🧤 Luvas", nome: "Luva Coral Resistência a Cortes e Furos até 350º - Danny", ca: "15.366", imagem: "https://i.imgur.com/be6AUx2.png" },
  { categoria: "🧤 Luvas", nome: "Luva de Segurança Confort Térmica Látex com Forro para Limpeza - Danny", ca: "15.532", imagem: "https://i.imgur.com/eSHbHzh.png" },
  { categoria: "🧤 Luvas", nome: "Luva de Látex Cano Longo Longatex - Danny", ca: "9.567", imagem: "https://i.imgur.com/bTvGvC9.png" },
  { categoria: "🧤 Luvas", nome: "Luva Resistente ao Corte Nível 5 Cut Smart - Volk", ca: "47.068", imagem: "https://i.imgur.com/ZlWIPQb.png" },
  { categoria: "🧤 Luvas", nome: "Luva Hand Nítrilo Lona - Handex", ca: "44.524", imagem: "https://i.imgur.com/fcOGOPT.png" },
  { categoria: "🧤 Luvas", nome: "Luva Térmica Frigorífica em Nylon Baixa Temperatura -35º - Maicol", ca: "10.978", imagem: "https://safetytrab.com.br/wp-content/uploads/2018/04/Luva-de-Seguran%C3%A7a-T%C3%A9rmica-em-Nylon-Maicol-CA-10.978.jpg.webp" },
  { categoria: "🧤 Luvas", nome: "Luva Hand Oil Cut - Handex", ca: "39.416", imagem: "https://i.imgur.com/50GGoXw.png" },
  { categoria: "🦾 Mangotes", nome: "Mangote Anti-Corte e Térmico 45cm - Delta Plus", ca: "41.361", imagem: "https://www.americanvek.com.br/cdn/shop/files/mangote-de-protecao-40cm-anticorte-nivel-5-com-fio-de-aco-seiki-ca39062-peca-1172807310_500x246.jpg?v=1749676771" },
  { categoria: "🦾 Mangotes", nome: "Mangote de Raspa Soldador com Elásticos 40cm - Zanel", ca: "16.073", imagem: "https://www.ferpam.com.br/media/mf_webp/jpg/media/catalog/product/cache/7f3660905effcfdd27a3ab16f16ab037/t/_/t_redu_o_13_-compressed.webp" },
  { categoria: "🦺 Aventais", nome: "Avental de Raspa 120x60cm sem Emendas - Zanel", ca: "13.989", imagem: "https://imgs.search.brave.com/0Zf1kLnKjFiMTVXbOqNhkYkugcJFzkH7wcKaLdcJIxg/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly9kM2Jo/dno3YWwzN2l5Ni5j/bG91ZGZyb250Lm5l/dC9DdXN0b20vQ29u/dGVudC9Qcm9kdWN0/cy8xMC82Ni8xMDY2/OTUzX2F2ZW50YWwt/ZW0tcmFzcGEtMTIw/eDcwLWNtLXphbmVs/LWF2LTEyMDcwc2Ut/c2VtLWVtZW5kYXMt/Y29tLXRpcmFzLWVt/LXJhc3BhLWUtZml2/ZWxhcy1tZXRhbGlj/YXMtY2EtMTM5ODlf/bDFfNjM4MjEyMzc3/NTY1MjE0NzUyLndl/YnA" },
  { categoria: "🦺 Aventais", nome: "Avental de PVC Branco - Maicol", ca: "37.729", imagem: "https://imgs.search.brave.com/Tl0cjZhWvJU0qdYADZvGZzDa4lv8fAXe1oUrvqYtL-Y/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly93d3cu/YXN0cm9kaXN0cmli/dWlkb3JhLmNvbS9t/ZWRpYS90bXAvd2Vi/cC9jYXRhbG9nL3By/b2R1Y3QvY2FjaGUv/MS9pbWFnZS82MDB4/LzlkZjc4ZWFiMzM1/MjVkMDhkNmU1ZmI4/ZDI3MTM2ZTk1L2Ev/di9hdmVudGFsX2Rl/X3B2Y19jb21fZm9y/cm9fMV8xNV94XzBf/NjVfY21fYnJhbmNv/XzBfMzBfbW1fLV9t/YWljb2xfY2FfLV8z/NzcyOV80XzIud2Vi/cA" },
  { categoria: "🦺 Aventais", nome: "Avental De Proteção De PVC Cores Balask - Branco", ca: "6.429", imagem: "https://http2.mlstatic.com/D_NQ_NP_786060-MLU76630545889_052024-O.webp" },
  { categoria: "🦺 Aventais", nome: "Avental De Proteção De PVC Cores Balask - Preto", ca: "6.429", imagem: "https://http2.mlstatic.com/D_NQ_NP_729531-MLU72636555305_112023-O.webp" },
  { categoria: "🦺 Aventais", nome: "Avental de Vinil Transparente Tira Soldada", ca: "38.316", imagem: "https://safetytrab.com.br/wp-content/uploads/2023/01/Avental-de-Vinil-Transparente-com-Fivela-Maicol-CA-38316-516x516.png" },
  { categoria: "😷 Máscaras", nome: "Máscara PFF1 com ProSafety - Delta Plus", ca: "38.501", imagem: "https://imgs.search.brave.com/DxGkuYzi-TIuHYfvbCkTB8x9QKqlQhpQQ-y0PQ0o2kk/rs:fit:500:0:1:0/g:ce/aHR0cHM6Ly9kM2Jo/dno3YWwzN2l5Ni5j/bG91ZGZyb250Lm5l/dC9DdXN0b20vQ29u/dGVudC9Qcm9kdWN0/cy8xMC80OC8xMDQ4/OTA2X21hc2NhcmEt/cGZmMS1jb20tdmFs/dnVsYS1wcm8tYWdy/by1kZWx0YS1wbHVz/LWNhaXhhLWNvbS0x/MDBfbTlfNjM3MzU5/NjY3MzQyNzE0NjUz/LndlYnA" },
  { categoria: "😷 Máscaras", nome: "Máscara N95 PF2 - Nutriex Safety", ca: "46.868", imagem: "https://imgs.search.brave.com/rVZe6d37VK0knK_LSb7UFBTtuzXH6Qqo2R5MgyT5oaw/rs:fit:500:0:1:0/g:ce/aHR0cHM6Ly9odHRw/Mi5tbHN0YXRpYy5j/b20vRF9OUV9OUF84/NDk5NDctTUxCNTI0/NzgxMjI3OTNfMTEy/MDIyLVYud2VicA" },
  { categoria: "😷 Máscaras", nome: "Máscara PFF2 - Delta Plus", ca: "38.503", imagem: "https://imgs.search.brave.com/ArpzaLtlElyrzt2dTe03WbqKAxJPbC_SIKyTP1WDyE0/rs:fit:500:0:1:0/g:ce/aHR0cHM6Ly9jZG4u/bGVyb3ltZXJsaW4u/Y29tLmJyL3Byb2R1/Y3RzL21hc2NhcmFf/ZGVzY2FydGF2ZWxf/cGZmMl9zX19jX192/YWx2dWxhX19kZWx0/YV9wbHVzXzkwNTk5/NTIzX2IwZjVfNjAw/eDYwMC5qcGc" },
  { categoria: "📌 Epis", nome: "Creme Protetor Luva Quimica 3em1 - Nutriex", ca: "43.802", imagem: "https://www.astrodistribuidora.com/media/tmp/webp/catalog/product/cache/1/image/600x/9df78eab33525d08d6e5fb8d27136e95/s/a/sabonete_desengraxante_esfoliante_limpa_m_os_biodegrad_vel_fast_orange_bombona_4l_-_luvex_img_1_1__png.webp" },
  { categoria: "📌 Epis", nome: "Cinturao de Segurança Steelflex com 1 ponto CQCT1111 + Talabarte Duplo em Y Com Fita Tubular", ca: "45.069", imagem:"https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/53/1053591_cinturao-de-seguranca-steelflex-com-1-ponto-cqct1111-talabarte-duplo-em-y-com-fita-tubular-_m3_637826022890604517.webp" },
  { categoria: "📌 Epis", nome: "Macacão de Seguranca Branco - SteelFlex", ca: "39.707", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/48/1048195_macacao-de-seguranca-steelflex-branco-ca-39707_m3_637358851517696820.webp" },
  { categoria: "📌 Epis", nome: "Luva Vinil Sem Pó Descartavel c/100 - Descarpack", ca: "44.050", imagem: "https://loja.descarpack.com.br/media/catalog/product/l/u/luva-vinil-procedimento-nao-cirurgico-sem-po-p-descarpack-0541101-1-para-que-indicado_2.jpg?auto=webp&format=pjpg&width=1600&height=2000&fit=cover" },
  { categoria: "🧥 Vestuário Térmico", nome: "Japona Térmica Frigorífica Azul Marinho - Maicol", ca: "10.975", imagem: "https://safetytrab.com.br/wp-content/uploads/2018/04/Japona-Termica-Camara-Fria-Baixa-Temperatura-Maicol-CA-10975.jpg.webp" },
  { categoria: "🧥 Vestuário Térmico", nome: "Calça De Nylon Térmica Impermeável Para Câmara Fria - Maicol", ca: "10.976", imagem: "https://safetytrab.com.br/wp-content/uploads/2018/04/Calca-Nylon-Termica-Camara-Fria-Baixa-Temperatura-Maicol-CA-10976-516x516.jpg" },
  { categoria: "🧥 Vestuário Térmico", nome: "Capuz Balaclava Térmico para Câmara Fria Suedine - Maicol", ca: "10.979", imagem: "https://images.tcdn.com.br/img/img_prod/626581/capuz_balaclava_suedine_maicol_1063_variacao_6239_1_af993d65a8dc368e7488518fa0726ba8.png" },
  { categoria: "🧥 Vestuário Térmico", nome: "Meião Térmico - Maicol", ca: "10.977", imagem: "https://safetytrab.com.br/wp-content/uploads/2018/04/Mei%C3%A3o-T%C3%A9rmico-para-C%C3%A2mara-Fria-Maicol.jpg.webp" },
  { categoria: "🧥 Vestuário", nome: "Camisa Com Refletivo Para Eletricista Cinza - Maicol", ca: "44.108", imagem: "https://safetytrab.com.br/wp-content/uploads/2023/06/Camisa-com-Refletivo-Eletricista-NR10-Cinza-Maicol-CA-44108.png" },
  { categoria: "🧥 Vestuário", nome: "Calça Classe 2 Cinza Com Refletivo - Maicol", ca: "44.109", imagem: "https://safetytrab.com.br/wp-content/uploads/2023/06/Calca-com-Refletivo-para-Eletricista-NR10-Cinza-Maicol-CA-44109-516x516.png" },
  { categoria: "🧥 Vestuário", nome: "Blusão PVC Forrado Com Capuz", ca: "29.790", imagem: "https://www.ledan.com.br/slideWF/images/calca-e-blusao-pvc-forrado/calca-e-blusao-pvc-forrado1.jpg" },
  { categoria: "🧥 Vestuário", nome: "Calça De Chuva Em PVC Forrada Amarela", ca: "37.536", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/46/1046987_calca-de-chuva-em-pvc-forrada-amarela-ca-28191-_z4_637669591007837495.webp" },
  { categoria: "🦺 Colete", nome: "Colete Tipo X Laranja Steelflex", ca: "38.175", imagem: "https://www.steelflex.pro/wp-content/uploads/2021/08/COLETE-REFLETIVO-X1.png" },
  { categoria: "🦺 Colete", nome: "Colete SteelFlex Refletivo Laranja Fluorescente", ca: "42.716", imagem: "https://www.steelflex.pro/wp-content/uploads/2021/08/colete-refletivo-4-bolsos-laranja.png" },
  { categoria: "🛡 Proteção Facial", nome: "Protetor Facial Jabre Carneira Hipoalergênica com Regulagem por Catraca Tamanho 8 - Delta Plus", ca: "47.620", imagem: "https://images.tcdn.com.br/img/img_prod/1033319/protetor_facial_jabre_8_delta_plus_ca_47620_1699_1_04f95abaf05adc053f1561dba26a2d78.jpg" },
  { categoria: "🛡 Proteção Facial", nome: "Protetor Facial com Carneira Hipoalergênica com regulagem e fácil ajuste - Delta Plus", ca: "10.975", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/67/1067058_protetor-facial-jabre-delta-plus-carneira-hipoalergenica-com-regulagem-ajuste-facil-tamanho-8-ca-47620_z1_638227684320944177.webp" },
  { categoria: "🛡 Proteção Facial", nome: "Protetor Facial Telado Dystray - Ideal Para Roçadeira", ca: "36.802", imagem: "https://elastobor.vtexassets.com/arquivos/ids/213582/PROTETOR-F%EF%BF%BDCIL-DYSTRAY-TELADO-COM-CATRACA.jpg?v=637557443744600000" },
  { categoria: "👓 Óculos", nome: "Óculos de Segurança Ampla Visão - Galeras Clear", ca: "35.268", imagem: "https://hiperfer.cdn.magazord.com.br/img/2023/07/produto/13687/14734-1-oculos-de-seguranca-com-ampla-visao-galeras-clear-deltaplus.jpg?ims=500x500" },
  { categoria: "👓 Óculos", nome: "Óculos Proteção Sobrepor Antirrisco Hekla Incolor - Delta Plus", ca: "38.253", imagem: "https://ccp.vteximg.com.br/arquivos/ids/240080-535-535/oculos-de-proteco-delta-plus-hekla-clear-ca-38253-D_NQ_NP_873213-MLB26641061700_012018-F.jpg?v=636843803319300000" },
  { categoria: "👓 Óculos", nome: "Óculos de Segurança tipo RJ Vvision 100 incolor - Volk", ca: "42.716", imagem: "https://volkdobrasil.com.br/wp-content/uploads/2024/08/oculos-vvision100-incolor-600x600.jpg" },
  { categoria: "🎧 Proteção Auditiva", nome: "Abafador de Ruído Combat 10DB - Maicol", ca: "10.977", imagem: "https://images.tcdn.com.br/img/img_prod/860085/abafador_concha_combat_10db_prosafety_ca_19405_175_1_20201214023621.jpg" },
  { categoria: "🎧 Proteção Auditiva", nome: "Abafador de Ruído SoftSlim 18DB - Camper", ca: "33.135", imagem: "https://images.tcdn.com.br/img/img_prod/652260/abafador_de_ruido_18_db_soft_slim_cod_800200_camper_5653_1_829cc44eecca796fb5e0ca7592040551_20230823104822.jpg" },
  { categoria: "🎧 Proteção Auditiva", nome: "Abafador de Ruído ConfortPlus 26DB - Camper", ca: "48.054", imagem: "https://images.tcdn.com.br/img/img_prod/652260/abafador_de_ruidos_26_db_confort_plus_camper_4258_1_29963934d741bf4bf802579cf35404ff_20230823104818.jpg" },
  { categoria: "🎧 Proteção Auditiva", nome: "Abafador de Ruído Combat 10DB - Delta Plus", ca: "19.405", imagem: "https://cdn.leroymerlin.com.br/products/abafador_de_ruido_combat_delta_plus_90599383_893d_600x600.jpeg" },
  { categoria: "🎧 Proteção Auditiva", nome: "Abafador Concha Interlagos 23DB - Delta Plus", ca: "35.003", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/51/1051099_abafador-concha-interlagos-cz-delta-plus-intergr-ca-35003_z27_638303840084340681.webp" }
 ];
  
  const catalog = document.getElementById('catalog');
  const categoryFilter = document.getElementById('categoryFilter');
  function renderCatalog(list) {
  catalog.innerHTML = list.map(epi => `
    <div class="item">
      <img src="${epi.imagem}" alt="${epi.nome}" />
      <div>${epi.categoria}</div>
      <h3>${epi.nome}</h3>
      <p>
        <strong>CA Nº</strong>: ${epi.ca}
        <button class="action" style="margin-left:8px" onclick="copiarTexto('${epi.ca}')">Copiar</button>
      </p>
    </div>`).join('');
}

  function populateCategoryFilter() {
    const categories = [...new Set(epis.map(e => e.categoria))];
    categories.forEach(cat => {
      const option = document.createElement('option');
      option.value = cat; option.textContent = cat;
      categoryFilter.appendChild(option);
    });
  }
  function searchItems() {
    const query = document.getElementById('searchInput').value.toLowerCase();
    const selectedCategory = categoryFilter.value;
    const filtered = epis.filter(e =>
      (e.nome.toLowerCase().includes(query) || e.ca.includes(query)) &&
      (selectedCategory === '' || e.categoria === selectedCategory)
    );
    renderCatalog(filtered);
  }
  populateCategoryFilter();
  renderCatalog(epis);

  /* -------- Filtro Avaliação de Riscos -------- */
  function filtrarRiscos() {
    const filtro = document.getElementById("filtro").value;
    document.querySelectorAll("#riscos .bloco").forEach(bloco => {
      const tipo = bloco.getAttribute("data-risco");
      bloco.style.display = (filtro === "todos" || filtro === tipo) ? "block" : "none";
    });
  }

  /* -------- Frases por Empresa -------- */
  function gerarFrases() {
    const nomes = document.getElementById("nomes").value.trim().split("\n").filter(Boolean);
    const empresa = document.getElementById("empresa").value.trim();
    const data = document.getElementById("data").value.trim();
    document.getElementById("resultado").innerText =
      nomes.map(nome => `${nome}, ${empresa}, , , ${data}`).join("\n");
  }

  /* -------- Riscos por Empresa (dinâmico) -------- */
  const riscosEmpresas = [
    { emoji:"🔊", risco:"RUÍDO", empresa:"FAP INDÚSTRIA E COMÉRCIO DE ACRÍLICOS LTDA", setor:"Dobra" },
    { emoji:"🌫", risco:"POEIRA", empresa:"FAP INDÚSTRIA E COMÉRCIO DE ACRÍLICOS LTDA", setor:"Polimento" },
    { emoji:"🛠", risco:"FERRAMENTAS MANUAIS/MÁQUINAS E EQUIPAMENTOS", empresa:"FAP INDÚSTRIA E COMÉRCIO DE ACRÍLICOS LTDA", setor:"Produção" },
    { emoji:"🧼", risco:"PRODUTOS DOMISSANITÁRIOS DE LIMPEZA", empresa:"FAP INDÚSTRIA E COMÉRCIO DE ACRÍLICOS LTDA", setor:"Limpeza" },
    { emoji:"👷‍♂️", risco:"TRABALHO EM ALTURA", empresa:"JF CONSTRUCAO E PAVIMENTAÇÃO LTDA", setor:"Obra" },
    { emoji:"🧺", risco:"TINTAS/DILUENTES", empresa:"PREST-MAC COMERCIAL E INDUSTRIAL LTDA", setor:"Pintura" },
    { emoji:"⚡", risco:"TRABALHO COM ELETRICIDADE", empresa:"WGL SOLUÇÕES EM TECNOLOGIAS E SERVIÇOS LTDA", setor:"Manutenção/Elétrica (Preventiva/Corretiva)" },
    { emoji:"🧴", risco:"LUBRIFICANTES", empresa:"PREST-MAC COMERCIAL E INDUSTRIAL LTDA", setor:"Produção" },
    { emoji:"✨", risco:"PRODUTOS QUÍMICOS DE ESTÉTICA", empresa:"ELENICE GERALDO DOS SANTOS CABELEIREIROS LTDA", setor:"Salão" },
    { emoji:"🍔", risco:"MANIPULAÇÃO DE ALIMENTOS", empresa:"PÃES E DOCES CHALÉ DOCILE LTDA", setor:"Cozinha" },
    { emoji:"🚗", risco:"CONDUÇÃO DE VEÍCULOS AUTOMOTORES", empresa:"POLICOMP COMERCIO DE COMPONENTES", setor:"Estoque/Transporte" },
    { emoji:"🔩", risco:"FUMOS METÁLICOS DE ESTANHO", empresa:"POLICOMP COMERCIO DE COMPONENTES", setor:"Técnico" },
    { emoji:"💥", risco:"RADIAÇÃO NÃO IONIZANTE", empresa:"SPADA MIDIA E EVENTOS LTDA", setor:"Obra/Serralheria" },
    { emoji:"😸🐶", risco:"PRODUTOS QUÍMICOS DE BELEZA PARA CÃES E GATOS", empresa:"CAROL PET SHOP COMERCIO DE RACOES ACESSORIOS BANHO E TOSA LTDA", setor:"Banho e Tosa" },
    { emoji:"📦", risco:"TRABALHOS DE SEPARAÇÃO DE EMBALAGENS", empresa:"MONTE VIRGINNE COMÉRCIO DE SUCATAS DE PLÁSTICOS LTDA ME", setor:"Produção/Separação" },
    { emoji:"❄", risco:"FRIO", empresa:"LANCHONETE E ESFIHARIA NOVA ALIANCA LTDA", setor:"Cozinha" },
    { emoji:"🧴", risco:"ÓLEO SINTÉTICO", empresa:"DELTA ROTULADORAS EIRELI", setor:"Fábrica" },
    { emoji:"🗜", risco:"ESTANHO", empresa:"RAUL SANTOS FERREIRA", setor:"Produção" }
  ];

  function renderRiscosEmpresas(list) {
    const container = document.getElementById("listaRiscosEmpresas");
    container.innerHTML = list.map(item => {
      const textoCopiar = `${item.risco} – ${item.empresa} – Setor ${item.setor}`;
      return `
        <div class="bloco">
          <p><strong>${item.emoji} ${item.risco}</strong><br>
          <small>${item.empresa} – Setor ${item.setor}</small></p>
          <button class="action" onclick="copiarTexto('${textoCopiar}')">Copiar</button>
        </div>`;
    }).join('');
  }
  function filtrarRiscosEmpresas() {
    const termo = document.getElementById("buscaRiscos").value.toLowerCase();
    const filtrados = riscosEmpresas.filter(i =>
      (i.risco + ' ' + i.empresa + ' ' + i.setor).toLowerCase().includes(termo)
    );
    renderRiscosEmpresas(filtrados);
  }
  renderRiscosEmpresas(riscosEmpresas);
</script>

</body>
</html>
