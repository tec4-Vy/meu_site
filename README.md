<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Painel Técnico - Segurança do Trabalho</title>

<!-- ====== Bibliotecas para exportação ====== -->
<!-- SheetJS para Excel -->
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js" defer></script>
<!-- jsPDF + autoTable para PDF -->
<script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js" defer></script>
<script src="https://cdn.jsdelivr.net/npm/jspdf-autotable@3.8.2/dist/jspdf.plugin.autotable.min.js" defer></script>

<style>
  /* ====== ESTILO BASE MELHORADO ====== */
  :root{
    --bg:#f5f7fb;
    --fg:#111827;
    --muted:#6b7280;
    --primary:#2c3e50;
    --primary-700:#1f2b39;
    --nav:#34495e;
    --card:#ffffff;
    --border:#e5e7eb;
    --accent:#6366f1;
    --ok:#22c55e;
    --warn:#f59e0b;
    --err:#ef4444;
    --shadow:0 10px 20px rgba(0,0,0,.08), 0 6px 6px rgba(0,0,0,.06);
  }
  *{box-sizing:border-box}
  body { font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; margin:0; background: var(--bg); color:var(--fg); }
  header { background: linear-gradient(135deg, var(--primary), #1d2935); color: #fff; padding: 22px 16px; text-align: center; position: sticky; top:0; z-index: 50; box-shadow: var(--shadow); }
  header h1{ margin:0; font-size: clamp(1.2rem, 2.5vw, 1.6rem); letter-spacing:.3px }
  nav {
    background: var(--nav);
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap:4px;
    padding: 6px;
    position: sticky;
    top:72px;
    z-index: 49;
  }
  nav button {
    background: rgba(255,255,255,.06); border: 1px solid rgba(255,255,255,0.12); color: #fff; padding: 10px 14px;
    font-size: .95rem; cursor: pointer; transition: transform .08s ease, background .2s ease, border-color .2s ease;
    border-radius: 10px;
  }
  nav button:hover { background: rgba(255,255,255,.14); transform: translateY(-1px); }
  nav button.active { background: #111827; border-color: rgba(255,255,255,.25); }

  .theme-toggle {
    position: absolute; right: 12px; top: 12px;
    background: #1f2937; color: #fff; border: 1px solid rgba(255,255,255,0.25);
    padding: 8px 12px; border-radius: 8px; cursor: pointer; font-size: 0.9rem;
  }
  .theme-toggle:hover { filter: brightness(1.1); }

  main{ max-width: 1100px; margin: 18px auto; padding: 0 14px; }
  section { display: none; }
  section.active { display: block; }

  h2 { color: var(--primary); border-bottom: 2px solid var(--primary); padding-bottom: 6px; font-size: 1.15rem; display:flex; align-items:center; gap:10px }
  h2 .tools{ margin-left:auto; display:flex; gap:8px; }

  .action {
    background-color: var(--primary); color: white; padding: 9px 14px;
    font-size: 0.9rem; border: none; border-radius: 8px; cursor: pointer;
    transition: transform .08s ease, background-color 0.2s ease, box-shadow .2s;
    box-shadow: var(--shadow);
  }
  .action:hover { background-color: var(--primary-700); transform: translateY(-1px); }

  textarea, input, select {
    width: 100%; padding: 11px 12px; margin-bottom: 14px;
    border: 1px solid var(--border); border-radius: 10px; font-size: 1rem; background:var(--card); color:var(--fg);
    box-shadow: 0 1px 0 rgba(0,0,0,.02);
  }

  .output, .bloco {
    background: var(--card); padding: 16px; border-radius: 12px;
    box-shadow: var(--shadow); margin-bottom: 16px; border:1px solid var(--border);
  }
  .bloco h3{ margin-top:0 }

  .bloco.fisico { border-left: 6px solid #3b82f6; }
  .bloco.biologico { border-left: 6px solid #22c55e; }
  .bloco.quimico { border-left: 6px solid #ef4444; }

  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 16px; }
  .item { background: var(--card); border-radius: 14px; padding: 14px; text-align: center; box-shadow: var(--shadow); border:1px solid var(--border); display:flex; flex-direction:column; gap:8px }
  .item img { width: 100%; max-width: 140px; height: auto; margin: 0 auto 6px; border-radius:10px; object-fit: cover; }
  .item h3{ font-size:1rem; margin:4px 0 0 0 }
  .muted{ color: var(--muted); font-size:.9rem }
  footer { text-align: center; font-size: 0.8rem; color: #555; padding: 12px; background: #eaeaea; margin-top: 26px; border-top:1px solid var(--border) }
  small { color: #6b7280; }

  /* ====== BADGES ====== */
  .badge{
    display:inline-flex; align-items:center; gap:6px; padding:4px 8px; border-radius: 999px;
    background: #eef2ff; color:#3730a3; border:1px solid #e0e7ff; font-size:.8rem
  }

  /* ====== TEMA ESCURO (override mínimo) ====== */
  body.dark { --bg:#0f1115; --fg:#e5e7eb; --muted:#9ca3af; --primary:#111827; --primary-700:#0b1220; --nav:#1f2937; --card:#111827; --border:#1f2937; --shadow: 0 8px 20px rgba(0,0,0,.45), 0 2px 6px rgba(0,0,0,.35); }
  body.dark header { background: linear-gradient(135deg, #111827, #0b1220); color: #e5e7eb; }
  body.dark nav { background: #1f2937; }
  body.dark nav button { color: #e5e7eb; border-color: rgba(255,255,255,.1) }
  body.dark nav button:hover, body.dark nav button.active { background: #111827; }

  body.dark h2 { color: #e5e7eb; border-color: #374151; }
  body.dark textarea, body.dark input, body.dark select { background: #111827; color: #e5e7eb; border-color: #374151; }
  body.dark .output, body.dark .bloco, body.dark .item { background: #111827; color: #e5e7eb; }
  body.dark small, body.dark .muted { color: #9ca3af; }
  body.dark footer { background: #0f1115; color: #9ca3af; border-top-color:#111827 }

  body.dark .bloco.fisico { border-left-color: #3b82f6; }
  body.dark .bloco.biologico { border-left-color: #22c55e; }
  body.dark .bloco.quimico { border-left-color: #ef4444; }

  /* ====== TOASTS ====== */
  .toast{
    position: fixed; right: 16px; bottom: 16px; background: #111827; color:#e5e7eb; padding: 12px 14px; border-radius: 10px; box-shadow: var(--shadow); opacity:0; transform: translateY(8px);
    transition: opacity .2s, transform .2s; z-index: 80; border:1px solid rgba(255,255,255,.12)
  }
  .toast.show{ opacity:1; transform: translateY(0) }
  .toast.ok{ background:#065f46 } /* verde escuro */
  .toast.err{ background:#7f1d1d } /* vermelho escuro */

  /* ====== MODAL (Senha) ====== */
  .modal-backdrop{
    position: fixed; inset:0; background: rgba(0,0,0,.45); display:none; align-items:center; justify-content:center; z-index: 90; padding: 16px;
  }
  .modal{
    width: 100%; max-width: 420px; background: var(--card); border:1px solid var(--border); border-radius: 14px; box-shadow: var(--shadow); padding: 18px;
  }
  .modal h3{ margin:0 0 8px 0 }
  .modal-actions{ display:flex; gap:10px; justify-content:flex-end; }

  /* ====== UTIL ====== */
  .row{ display:flex; gap:10px; flex-wrap:wrap }
  .row > *{ flex: 1 1 200px }
  .hint{ font-size:.85rem; color:var(--muted) }
</style>
</head>
<body>

<header>
  <h1>Painel Técnico - Segurança do Trabalho</h1>
  <button class="theme-toggle" onclick="toggleTheme()" aria-label="Alternar Tema">Alternar Tema</button>
</header>

<nav id="tabs" role="tablist" aria-label="Navegação principal">
  <button role="tab" aria-selected="true" class="active" onclick="mostrar('epis', this)">📒 Catálogo de EPIs</button>
  <button role="tab" aria-selected="false" onclick="mostrar('riscos', this, true)">🧪 Avaliação de Riscos</button>
  <button role="tab" aria-selected="false" onclick="mostrar('empresas', this)">🏢 Frases com Empresa</button>
  <button role="tab" aria-selected="false" onclick="mostrar('treinamento', this, true)">📚 Treinamento NR-06</button>
  <button role="tab" aria-selected="false" onclick="mostrar('riscos_empresas', this, true)">📋 Riscos por Empresa</button>
</nav>

<main>
  <!-- EPIs -->
  <section id="epis" class="active" aria-labelledby="tab-epis">
    <h2>
      Catálogo de EPIs
      <span class="tools">
        <button class="action" onclick="exportEpisExcel()" aria-label="Exportar EPIs para Excel">Exportar Excel</button>
        <button class="action" onclick="exportEpisPDF()" aria-label="Exportar EPIs para PDF">Exportar PDF</button>
      </span>
    </h2>
    <div class="row">
      <input type="text" id="searchInput" placeholder="Buscar por nome, CA ou categoria..." onkeyup="searchItems()" aria-label="Buscar EPIs por nome, CA ou categoria"/>
      <select id="categoryFilter" onchange="searchItems()" aria-label="Filtrar por categoria">
        <option value="">Todas as Categorias</option>
      </select>
    </div>
    <div class="grid" id="catalog" aria-live="polite"></div>
    <p class="hint">Dica: clique em “CA Nº” para copiar o número rapidamente.</p>
  </section>

  <!-- Avaliação de Riscos -->
  <section id="riscos" aria-labelledby="tab-riscos">
    <h2>
      Avaliação de Riscos
      <span class="tools">
        <button class="action" onclick="exportTextoRiscosPDF()">Exportar PDF</button>
      </span>
    </h2>
    <div class="row">
      <select id="filtro" onchange="filtrarRiscos()" aria-label="Filtro de riscos">
        <option value="todos">Todos</option>
        <option value="fisico">Físico</option>
        <option value="biologico">Biológico</option>
        <option value="quimico">Químico</option>
      </select>
    </div>
    <div id="conteudo">
      <!-- Físico -->
      <div class="bloco fisico" data-risco="fisico">
        <h3>📘 FÍSICO - Parte 1</h3>
        <p>De acordo com a inspeção realizada no ambiente de trabalho e atividades executadas pelo trabalhador que desempenha este cargo, e de acordo com a NR 15 da portaria 3.214/78 do M.T.E, o mesmo está exposto a agentes ambientais nocivos a saúde ao risco Físico.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
      <div class="bloco fisico" data-risco="fisico">
        <h3>📘 FÍSICO - Parte 2</h3>
        <p>As atividades de trabalho realizadas neste LTCAT não são consideradas de Condições Especiais de Trabalho e, portanto, não são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
      <!-- Biológico -->
      <div class="bloco biologico" data-risco="biologico">
        <h3>🧬 BIOLÓGICO - Parte 1</h3>
        <p>De acordo com a inspeção realizada no ambiente de trabalho e atividades executadas pelo trabalhador que desempenha este cargo, e de acordo com a NR 15 da portaria 3.214/78 do M.T.E, o mesmo está exposto a agentes ambientais nocivos a saúde ao risco Biológico.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
      <div class="bloco biologico" data-risco="biologico">
        <h3>🧬 BIOLÓGICO - Parte 2</h3>
        <p>As atividades de trabalho realizadas neste LTCAT não são consideradas de Condições Especiais de Trabalho e, portanto, não são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
      <div class="bloco biologico" data-risco="biologico">
        <h3>🧬 BIOLÓGICO - Parte 3</h3>
        <p>As atividades de trabalho realizadas neste LTCAT são consideradas de Condições Especiais de Trabalho e, portanto, são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
      <!-- Químico -->
      <div class="bloco quimico" data-risco="quimico">
        <h3>🧪 QUÍMICO - Parte 1</h3>
        <p>De acordo com a inspeção realizada no ambiente de trabalho e atividades executadas pelo trabalhador que desempenha este cargo, e de acordo com a NR 15 da portaria 3.214/78 do M.T.E, o mesmo está exposto a agentes ambientais nocivos a saúde ao risco Químico.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
      <div class="bloco quimico" data-risco="quimico">
        <h3>🧪 QUÍMICO - Parte 2</h3>
        <p>As atividades de trabalho realizadas neste LTCAT não são consideradas de Condições Especiais de Trabalho e, portanto, não são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
      <div class="bloco quimico" data-risco="quimico">
        <h3>🧪 QUÍMICO - Parte 3</h3>
        <p>As atividades de trabalho realizadas neste LTCAT são consideradas de Condições Especiais de Trabalho e, portanto, são prejudiciais à saúde ou integridade física dos trabalhadores segundo os requisitos do Decreto Federal 3048 / 1999 e seu Anexo IV.</p>
        <button class="action" data-copy>Copiar</button>
      </div>
    </div>
  </section>

  <!-- Frases com Empresa -->
  <section id="empresas" aria-labelledby="tab-empresas">
    <h2>Gerador de Frases com Empresa</h2>
    <textarea id="nomes" rows="6" placeholder="Um nome por linha"></textarea>
    <div class="row">
      <input type="text" id="empresa" placeholder="Nome da empresa" />
      <input type="text" id="data" placeholder="Data" />
    </div>
    <div class="row">
      <button class="action" onclick="gerarFrases()">Gerar Frases</button>
      <button class="action" onclick="exportFrasesTXT()">Exportar TXT</button>
    </div>
    <div class="output" id="resultado" aria-live="polite"></div>
  </section>

  <!-- Treinamento -->
  <section id="treinamento" aria-labelledby="tab-treinamento">
    <h2>Treinamento NR-06</h2>
    <div class="bloco"><h3>Parte 1</h3><p>Treinamento de NR-06</p><button class="action" data-copy>Copiar</button></div>
    <div class="bloco"><h3>Parte 2</h3><p>Coordenador da empresa</p><button class="action" data-copy>Copiar</button></div>
    <div class="bloco"><h3>Parte 3</h3><p>Permanente</p><button class="action" data-copy>Copiar</button></div>
    <div class="bloco"><h3>Parte 4</h3><p>Ambiente laboral</p><button class="action" data-copy>Copiar</button></div>
    <div class="bloco"><h3>Parte 5</h3><p>Conscientizar os colaboradores sobre o uso de EPI.</p><button class="action" data-copy>Copiar</button></div>
    <div class="bloco"><h3>Parte 6</h3><p>Palestra NR-06, treinamento de EPI.</p><button class="action" data-copy>Copiar</button></div>
  </section>

  <!-- Riscos por Empresa -->
  <section id="riscos_empresas" aria-labelledby="tab-riscos-empresas">
    <h2>
      Riscos por Empresa
      <span class="tools">
        <button class="action" onclick="exportRiscosExcel()">Exportar Excel</button>
        <button class="action" onclick="exportRiscosPDF()">Exportar PDF</button>
      </span>
    </h2>
    <input type="text" id="buscaRiscos" placeholder="Buscar risco, empresa ou setor..." onkeyup="filtrarRiscosEmpresas()" />
    <div id="listaRiscosEmpresas"></div>
  </section>
</main>

<footer>
  Desenvolvido para fins técnicos de SST | &copy; 2025
  &nbsp;•&nbsp; Desenvolvido por Vytor Suporte Técnico
</footer>

<!-- ===== Modal de Senha ===== -->
<div class="modal-backdrop" id="modalSenha" role="dialog" aria-modal="true" aria-labelledby="tituloModalSenha" aria-describedby="descModalSenha">
  <div class="modal">
    <h3 id="tituloModalSenha">Acesso restrito</h3>
    <p class="hint" id="descModalSenha">Informe a senha para acessar esta seção protegida.</p>
    <input type="password" id="inputSenha" placeholder="Digite a senha..." />
    <div class="modal-actions">
      <button class="action" onclick="fecharModalSenha()">Cancelar</button>
      <button class="action" onclick="confirmarSenha()">Confirmar</button>
    </div>
    <p id="erroSenha" class="hint" style="color:var(--err);margin:8px 2px 0 2px;display:none">Senha incorreta. Tente novamente.</p>
  </div>
</div>

<!-- ===== Toast ===== -->
<div class="toast" id="toast" role="status" aria-live="polite"></div>

<script>
  /* ===== Proteção por senha nas seções (com persistência de sessão e modal) ===== */
  const senhaCorreta = "SG393"; // a sua senha original
  const secoesProtegidas = ["riscos", "treinamento", "riscos_empresas"];
  let secaoSolicitada = null;

  function mostrar(id, btnEl, protegido = false) {
    // Protegida?
    if (secoesProtegidas.includes(id) || protegido) {
      // já autenticado nesta aba?
      if (sessionStorage.getItem("sst_authed") === "true") {
        ativarSecao(id, btnEl);
        return;
      }
      // abrir modal
      secaoSolicitada = { id, btnEl };
      abrirModalSenha();
      return;
    }
    ativarSecao(id, btnEl);
  }

  function ativarSecao(id, btnEl){
    document.querySelectorAll("section").forEach(el => el.classList.remove("active"));
    document.getElementById(id).classList.add("active");

    // estado ativo na navegação
    document.querySelectorAll("nav button").forEach(b => { b.classList.remove("active"); b.setAttribute("aria-selected","false"); });
    if(btnEl){ btnEl.classList.add("active"); btnEl.setAttribute("aria-selected","true"); }
  }

  function abrirModalSenha(){
    const m = document.getElementById('modalSenha');
    document.getElementById('erroSenha').style.display = 'none';
    document.getElementById('inputSenha').value = '';
    m.style.display = 'flex';
    setTimeout(()=>document.getElementById('inputSenha').focus(), 10);
  }
  function fecharModalSenha(){
    document.getElementById('modalSenha').style.display = 'none';
    secaoSolicitada = null;
  }
  function confirmarSenha(){
    const val = document.getElementById('inputSenha').value.trim();
    if(val === senhaCorreta){
      sessionStorage.setItem("sst_authed", "true");
      mostrar(secaoSolicitada.id, secaoSolicitada.btnEl);
      fecharModalSenha();
      showToast("Acesso liberado.", "ok");
    }else{
      document.getElementById('erroSenha').style.display = 'block';
      showToast("Senha incorreta.", "err");
    }
  }
  // ESC fecha modal
  document.addEventListener('keydown', (e)=>{ if(e.key==='Escape' && document.getElementById('modalSenha').style.display==='flex'){ fecharModalSenha(); } });

  /* ===== Utilitário de cópia com Toast ===== */
  async function copiarTexto(texto) {
    try {
      await navigator.clipboard.writeText(texto);
      showToast("Copiado!", "ok");
    } catch (e) {
      // fallback
      const ta = document.createElement('textarea');
      ta.value = texto;
      document.body.appendChild(ta);
      ta.select();
      document.execCommand('copy');
      document.body.removeChild(ta);
      showToast("Copiado!", "ok");
    }
  }
  // Delegação para botões com data-copy
  document.addEventListener('click', (ev)=>{
    const btn = ev.target.closest('button[data-copy]');
    if(btn){
      const p = btn.previousElementSibling; // parágrafo antes do botão
      if(p) copiarTexto(p.innerText);
    }
  });

  /* ===== Toast ===== */
  function showToast(msg, kind){
    const t = document.getElementById('toast');
    t.textContent = msg;
    t.className = 'toast show' + (kind ? ' ' + (kind==='ok' ? 'ok' : kind==='err' ? 'err' : '') : '');
    setTimeout(()=>{ t.classList.remove('show'); }, 1800);
  }

  /* -------- EPIs (LISTA ORIGINAL COMPLETA) -------- */
  const epis = [
    { categoria: "👢 Calçados", nome: "Botina de Elástico com Bico de PVC - Fujiwara", ca: "48.413", imagem: "https://alturaecia.com.br/wp-content/uploads/2022/03/Botina-de-Elastico-com-Bico-PVC-Usafe-Fujiwara.jpg" },
    { categoria: "👢 Calçados", nome: "Bota de Segurança Bico Composite NR10 Eletricista - Bracol", ca: "45.258", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/68/1068213_bota-seguranca-bracol-microfibra-composite-eletricista-38530_z2_638430758882818688.webp" },
    { categoria: "👢 Calçados", nome: "Sapato de Amarrar em Couro com Palmilha - Fujiwara (Branco/Preto)", ca: "41.858", imagem: "https://images.tcdn.com.br/img/img_prod/1033319/sapato_de_amarrar_fujiwara_linha_usafe_em_couro_com_palmilha_ca_41858_4098usas4600us_171_1_370dd8cc6d1594636c42615405c0579d.jpg" },
    { categoria: "👢 Calçados", nome: "Sapato Antiderrapante Impermeável - Steelflex", ca: "38.590", imagem: "https://btequipamentos.agilecdn.com.br/111067_1_1.jpg?v=220-858371529" },
    { categoria: "👢 Calçados", nome: "Bota de PVC Meio Cano (Branco/Preto) - Bracol", ca: "37.456", imagem: "https://lojaagrometal.fbitsstatic.net/img/p/bota-pvc-preto-cano-medio-39-com-forro-ca-36-025-innpro-73005/259515.jpg?w=1000&h=1000&v=no-change&qs=ignore" },
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
    { categoria: "🦺 Aventais", nome: "Avental de Vinil Transparente Tira Soldada", ca: "38.316", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/51/1051169_avental-de-vinil-transparente-tira-soldada-ca-37475_z1_638821262518773542.webp" },
    { categoria: "😷 Máscaras", nome: "Máscara PFF1 com ProSafety - Delta Plus", ca: "38.501", imagem: "https://imgs.search.brave.com/DxGkuYzi-TIuHYfvbCkTB8x9QKqlQhpQQ-y0PQ0o2kk/rs:fit:500:0:1:0/g:ce/aHR0cHM6Ly9kM2Jo/dno3YWwzN2l5Ni5j/bG91ZGZyb250Lm5l/dC9DdXN0b20vQ29u/dGVudC9Qcm9kdWN0/cy8xMC80OC8xMDQ4/OTA2X21hc2NhcmEt/cGZmMS1jb20tdmFs/dnVsYS1wcm8tYWdy/by1kZWx0YS1wbHVz/LWNhaXhhLWNvbS0x/MDBfbTlfNjM3MzU5/NjY3MzQyNzE0NjUz/LndlYnA" },
    { categoria: "😷 Máscaras", nome: "Máscara N95 PF2 - Nutriex Safety", ca: "46.868", imagem: "https://imgs.search.brave.com/rVZe6d37VK0knK_LSb7UFBTtuzXH6Qqo2R5MgyT5oaw/rs:fit:500:0:1:0/g:ce/aHR0cHM6Ly9odHRw/Mi5tbHN0YXRpYy5j/b20vRF9OUV9OUF84/NDk5NDctTUxCNTI0/NzgxMjI3OTNfMTEy/MDIyLVYud2VicA" },
    { categoria: "😷 Máscaras", nome: "Máscara PFF2 - Delta Plus", ca: "38.503", imagem: "https://imgs.search.brave.com/ArpzaLtlElyrzt2dTe03WbqKAxJPbC_SIKyTP1WDyE0/rs:fit:500:0:1:0/g:ce/aHR0cHM6Ly9jZG4u/bGVyb3ltZXJsaW4u/Y29tLmJyL3Byb2R1/Y3RzL21hc2NhcmFf/ZGVzY2FydGF2ZWxf/cGZmMl9zX19jX192/YWx2dWxhX19kZWx0/YV9wbHVzXzkwNTk5/NTIzX2IwZjVfNjAw/eDYwMC5qcGc" },
    { categoria: "📌 Epis", nome: "Creme Protetor Luva Quimica 3em1 - Nutriex", ca: "43.802", imagem: "https://www.astrodistribuidora.com/media/tmp/webp/catalog/product/cache/1/image/600x/9df78eab33525d08d6e5fb8d27136e95/s/a/sabonete_desengraxante_esfoliante_limpa_m_os_biodegrad_vel_fast_orange_bombona_4l_-_luvex_img_1_1__png.webp" },
    { categoria: "📌 Epis", nome: "Cinturao de Segurança Steelflex com 1 ponto CQCT1111 + Talabarte Duplo em Y Com Fita Tubular", ca: "45.069", imagem:"https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/53/1053591_cinturao-de-seguranca-steelflex-com-1-ponto-cqct1111-talabarte-duplo-em-y-com-fita-tubular-_m3_637826022890604517.webp" },
    { categoria: "📌 Epis", nome: "Macacão de Seguranca Branco - SteelFlex", ca: "39.707", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/48/1048195_macacao-de-seguranca-steelflex-branco-ca-39707_m3_637358851517696820.webp" },
    { categoria: "📌 Epis", nome: "Luva Vinil Sem Pó Descartavel c/100 - Descarpack", ca: "44.050", imagem: "https://loja.descarpack.com.br/media/catalog/product/l/u/luva-vinil-procedimento-nao-cirurgico-sem-po-p-descarpack-0541101-1-para-que-indicado_2.jpg?auto=webp&format=pjpg&width=1600&height=2000&fit=cover" },
    { categoria: "🧥 Vestuário Térmico", nome: "Japona Térmica Frigorífica Azul Marinho - Maicol", ca: "10.975", imagem: "https://safetytrab.com.br/wp-content/uploads/2018/04/Japona-Termica-Camara-Fria-Baixa-Temperatura-Maicol-CA-10975.jpg.webp" },
    { categoria: "🧥 Vestuário Térmico", nome: "Calça De Nylon Térmica Impermeável Para Câmara Fria - Maicol", ca: "10.976", imagem: "https://http2.mlstatic.com/D_NQ_NP_2X_948835-MLB78618348261_082024-F.webp" },
    { categoria: "🧥 Vestuário Térmico", nome: "Capuz Balaclava Térmico para Câmara Fria Suedine - Maicol", ca: "10.979", imagem: "https://images.tcdn.com.br/img/img_prod/626581/capuz_balaclava_suedine_maicol_1063_variacao_6239_1_af993d65a8dc368e7488518fa0726ba8.png" },
    { categoria: "🧥 Vestuário Térmico", nome: "Meião Térmico - Maicol", ca: "10.977", imagem: "https://safetytrab.com.br/wp-content/uploads/2018/04/Mei%C3%A3o-T%C3%A9rmico-para-C%C3%A2mara-Fria-Maicol.jpg.webp" },
    { categoria: "👷‍ Vestuário", nome: "Camisa Com Refletivo Para Eletricista Cinza - Maicol", ca: "44.108", imagem: "https://d1ql0a3e3j9mcx.cloudfront.net/Custom/Content/Products/57/86/5786_camisa-eletricista-nr10-cinza-com-refletivo-maicol_l1_637934046958088869.webp" },
    { categoria: "👷‍ Vestuário", nome: "Calça Classe 2 Cinza Com Refletivo - Maicol", ca: "44.109", imagem: "https://d1ql0a3e3j9mcx.cloudfront.net/Custom/Content/Products/58/38/5838_calca-eletricista-nr10-com-refletivo-maicol_z1_637934927655407307.webp" },
    { categoria: "👷‍ Vestuário", nome: "Blusão PVC Forrado Com Capuz", ca: "29.790", imagem: "https://www.ledan.com.br/slideWF/images/calca-e-blusao-pvc-forrado/calca-e-blusao-pvc-forrado1.jpg" },
    { categoria: "👷‍ Vestuário", nome: "Calça De Chuva Em PVC Forrada Amarela", ca: "37.536", imagem: "https://d3bhvz7al37iy6.cloudfront.net/Custom/Content/Products/10/46/1046987_calca-de-chuva-em-pvc-forrada-amarela-ca-28191-_z4_637669591007837495.webp" },
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
    catalog.innerHTML = list.map((epi, idx) => `
      <div class="item" role="article" aria-labelledby="epi-${idx}-title">
        <img src="${epi.imagem}" alt="${epi.nome}" loading="lazy" />
        <div class="muted">${epi.categoria}</div>
        <h3 id="epi-${idx}-title">${epi.nome}</h3>
        <p class="muted">
          <span class="badge" title="Certificado de Aprovação">CA Nº: <strong>${epi.ca}</strong></span>
        </p>
        <button class="action" onclick="copiarTexto('${epi.ca.replace(/'/g, "\\'")}')">Copiar CA</button>
      </div>`).join('');
  }

  function populateCategoryFilter() {
    const categories = [...new Set(epis.map(e => e.categoria))].sort((a,b)=>a.localeCompare(b));
    categories.forEach(cat => {
      const option = document.createElement('option');
      option.value = cat; option.textContent = cat;
      categoryFilter.appendChild(option);
    });
  }
  function searchItems() {
    const query = document.getElementById('searchInput').value.toLowerCase().trim();
    const selectedCategory = categoryFilter.value;
    const filtered = epis.filter(e =>
      (e.nome.toLowerCase().includes(query) || e.ca.includes(query) || e.categoria.toLowerCase().includes(query)) &&
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
    const saida = nomes.map(nome => `${nome}, ${empresa}, , , ${data}`).join("\n");
    document.getElementById("resultado").innerText = saida || "—";
    showToast("Frases geradas.", "ok");
  }
  function exportFrasesTXT(){
    const conteudo = document.getElementById("resultado").innerText || "";
    const blob = new Blob([conteudo], { type: "text/plain;charset=utf-8" });
    const url = URL.createObjectURL(blob);
    const a = Object.assign(document.createElement('a'), { href:url, download:'frases_empresa.txt' });
    document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
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
      const textoCopiar = `${item.risco} – ${item.empresa} – Setor ${item.setor}`.replace(/"/g, '&quot;');
      return `
        <div class="bloco">
          <p><strong>${item.emoji} ${item.risco}</strong><br>
          <small class="muted">${item.empresa} – Setor ${item.setor}</small></p>
          <div class="row">
            <button class="action" onclick="copiarTexto('${textoCopiar}')">Copiar</button>
          </div>
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

  /* ===== Tema escuro com memória (mantido e refinado) ===== */
  const THEME_KEY = 'sst_theme';
  const prefersDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;

  function applyTheme(theme) {
    document.body.classList.toggle('dark', theme === 'dark');
  }
  function toggleTheme() {
    const next = document.body.classList.contains('dark') ? 'light' : 'dark';
    applyTheme(next);
    try { localStorage.setItem(THEME_KEY, next); } catch(e){}
  }
  (function initTheme(){
    let saved = null;
    try { saved = localStorage.getItem(THEME_KEY); } catch(e){}
    applyTheme(saved ? saved : (prefersDark ? 'dark' : 'light'));
  })();

  /* ===== Exportações ===== */
  function exportEpisExcel(){
    const dados = epis.map(e => ({ Categoria:e.categoria, Nome:e.nome, CA:e.ca, Imagem:e.imagem }));
    const ws = XLSX.utils.json_to_sheet(dados);
    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, "EPIs");
    XLSX.writeFile(wb, "catalogo_epis.xlsx");
  }
  function exportEpisPDF(){
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation:'p', unit:'pt', format:'a4' });
    doc.setFontSize(14);
    doc.text("Catálogo de EPIs", 40, 40);
    const rows = epis.map(e => [e.categoria, e.nome, e.ca]);
    doc.autoTable({
      head:[["Categoria","Nome","CA"]],
      body: rows,
      startY: 60,
      styles: { fontSize: 9, cellPadding: 4 },
      headStyles: { fillColor: [44, 62, 80] }
    });
    doc.save("catalogo_epis.pdf");
  }
  function exportRiscosExcel(){
    const dados = riscosEmpresas.map(r => ({ Emoji:r.emoji, Risco:r.risco, Empresa:r.empresa, Setor:r.setor }));
    const ws = XLSX.utils.json_to_sheet(dados);
    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, "Riscos por Empresa");
    XLSX.writeFile(wb, "riscos_por_empresa.xlsx");
  }
  function exportRiscosPDF(){
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation:'p', unit:'pt', format:'a4' });
    doc.setFontSize(14);
    doc.text("Riscos por Empresa", 40, 40);
    const rows = riscosEmpresas.map(r => [r.emoji, r.risco, r.empresa, r.setor]);
    doc.autoTable({
      head:[["","Risco","Empresa","Setor"]],
      body: rows,
      startY: 60,
      styles: { fontSize: 9, cellPadding: 4 },
      headStyles: { fillColor: [44, 62, 80] }
    });
    doc.save("riscos_por_empresa.pdf");
  }
  function exportTextoRiscosPDF(){
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation:'p', unit:'pt', format:'a4' });
    doc.setFontSize(14);
    doc.text("Avaliação de Riscos - Textos", 40, 40);

    const blocos = Array.from(document.querySelectorAll('#conteudo .bloco')).map(b=>({
      titulo: b.querySelector('h3')?.innerText || '',
      texto:  b.querySelector('p')?.innerText || ''
    }));

    let y = 60;
    doc.setFontSize(11);
    blocos.forEach((b, idx)=>{
      if(y > 760){ doc.addPage(); y=40; }
      doc.setFont(undefined,'bold');
      doc.text(b.titulo, 40, y); y += 14;
      doc.setFont(undefined,'normal');
      const split = doc.splitTextToSize(b.texto, 515);
      doc.text(split, 40, y);
      y += split.length * 12 + 10;
    });
    doc.save("avaliacao_riscos_textos.pdf");
  }

  /* ===== Acessibilidade extra: Enter confirma modal ===== */
  document.getElementById('inputSenha').addEventListener('keydown', (e)=>{
    if(e.key === 'Enter'){ e.preventDefault(); confirmarSenha(); }
  });
</script>

</body>
</html>
