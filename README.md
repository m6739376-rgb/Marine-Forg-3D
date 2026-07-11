<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Marine Forge 3D</title>
  <style>
    :root {
      --bg: #05070d;
      --panel: #0f1524;
      --panel-2: #171f2f;
      --line: rgba(255,255,255,0.12);
      --text: #f3f7ff;
      --muted: #8ea0b8;
      --accent: #39d2b2;
      --accent-2: #6f8cff;
      --danger: #ff7b7b;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      color: var(--text);
      background: linear-gradient(135deg, #05070d, #0b1220 55%, #05070d);
      height: 100vh;
      overflow: hidden;
    }
    button, input, select, textarea { font: inherit; }
    .app { display: grid; grid-template-columns: 300px 1fr 340px; height: 100vh; }
    .sidebar, .properties-panel {
      background: rgba(7, 10, 18, 0.95);
      border-right: 1px solid var(--line);
      border-left: 1px solid var(--line);
      padding: 16px;
      display: flex;
      flex-direction: column;
      gap: 12px;
      overflow: auto;
    }
    .sidebar { border-left: none; }
    .brand {
      font-size: 0.93rem; letter-spacing: 0.28em; text-transform: uppercase; color: var(--accent); font-weight: 700;
      padding-bottom: 10px; border-bottom: 1px solid var(--line);
    }
    .sidebar-section { display: flex; flex-direction: column; gap: 8px; }
    .sidebar-title {
      font-size: 0.74rem; letter-spacing: 0.24em; text-transform: uppercase; color: var(--muted);
    }
    .btn {
      border: 1px solid var(--line); border-radius: 999px; padding: 8px 12px; background: rgba(255,255,255,0.05); color: var(--text);
      cursor: pointer; transition: transform .2s ease, border-color .2s ease;
    }
    .btn:hover { transform: translateY(-1px); border-color: rgba(255,255,255,0.2); }
    .btn.primary { background: linear-gradient(135deg, var(--accent), var(--accent-2)); color: #03101b; border: none; }
    .btn.danger { background: rgba(255,123,123,0.1); color: #ffd8d8; }
    .project-card {
      border: 1px solid var(--line); border-radius: 14px; padding: 10px 12px; background: rgba(255,255,255,0.04); cursor: pointer;
      display: flex; justify-content: space-between; align-items: center;
    }
    .project-card.active { border-color: rgba(57,210,178,0.42); background: rgba(57,210,178,0.12); }
    .project-card strong { display: block; margin-bottom: 4px; }
    .project-card span { color: var(--muted); font-size: 0.8rem; }
    .list { display: flex; flex-direction: column; gap: 8px; }
    .list-item { padding: 8px; border-radius: 10px; background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.05); }
    .list-item.active { border-color: rgba(57,210,178,0.36); }
    .library-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
    .library-btn {
      border: 1px solid var(--line); border-radius: 12px; background: rgba(255,255,255,0.04); color: var(--text); padding: 10px; cursor: pointer;
    }
    .library-btn:hover { border-color: rgba(57,210,178,0.35); }
    .main { display: flex; flex-direction: column; min-width: 0; }
    .topbar { height: 56px; display: flex; justify-content: space-between; align-items: center; padding: 0 16px; background: rgba(8,12,22,0.95); border-bottom: 1px solid var(--line); }
    .menu-group { display: flex; gap: 8px; flex-wrap: wrap; }
    .menu-btn { background: transparent; color: var(--muted); border: 1px solid var(--line); border-radius: 10px; padding: 7px 10px; cursor: pointer; }
    .menu-btn:hover { color: var(--text); }
    .topbar-right { display: flex; align-items: center; gap: 10px; }
    .save-pill { color: var(--muted); font-size: 0.9rem; }
    .empty-state { flex: 1; display: flex; align-items: center; justify-content: center; padding: 24px; }
    .empty-card {
      width: min(760px, 100%); background: rgba(17,24,38,0.95); border: 1px solid var(--line); border-radius: 24px; padding: 28px;
      box-shadow: 0 18px 48px rgba(0,0,0,0.28); display: flex; flex-direction: column; gap: 14px; align-items: center; text-align: center;
    }
    .empty-card h2 { margin: 0; font-size: 1.35rem; }
    .empty-card p { margin: 0; color: var(--muted); line-height: 1.7; max-width: 680px; }
    .viewport-shell { flex: 1; display: flex; flex-direction: column; min-height: 0; }
    .toolbar { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; padding: 8px 10px; background: rgba(8,12,22,0.95); border-bottom: 1px solid var(--line); }
    .pill { padding: 6px 10px; border-radius: 999px; background: rgba(255,255,255,0.06); color: var(--muted); font-size: 0.84rem; }
    #viewport { flex: 1; position: relative; }
    .panel-card { border: 1px solid var(--line); border-radius: 16px; background: rgba(255,255,255,0.04); padding: 12px; }
    .panel-card h3 { margin: 0 0 8px; font-size: 0.95rem; }
    .field { display: flex; flex-direction: column; gap: 6px; margin-bottom: 8px; }
    .field label { font-size: 0.82rem; color: var(--muted); }
    .field input, .field select { width: 100%; border: 1px solid var(--line); border-radius: 10px; padding: 8px 10px; background: rgba(255,255,255,0.05); color: var(--text); }
    .field input[type=color] { padding: 2px; height: 36px; }
    .hidden { display: none !important; }
    @media (max-width: 1180px) {
      .app { grid-template-columns: 1fr; }
      .sidebar, .properties-panel { display: none; }
    }
  </style>
</head>
<body>
  <div class="app">
    <aside class="sidebar">
      <div class="brand">Marine Forge</div>
      <div class="sidebar-section">
        <div class="sidebar-title">Projets</div>
        <button class="btn primary" id="newProjectBtn">+ Nouveau projet</button>
        <div id="projectList" class="list"></div>
      </div>
      <div class="sidebar-section">
        <div class="sidebar-title">Créer</div>
        <button class="btn" id="createHullBtn">Créer coque</button>
        <button class="btn" id="editHullBtn">Modifier forme</button>
        <button class="btn" id="generateHullBtn">Générer coque</button>
        <button class="btn" id="addDeckBtn">Ajouter pont</button>
        <button class="btn" id="addCabinBtn">Ajouter cabine</button>
        <button class="btn" id="addMastBtn">Ajouter mât</button>
        <button class="btn" id="addSailBtn">Ajouter voile</button>
        <button class="btn" id="addPropellerBtn">Ajouter hélice</button>
        <button class="btn" id="addRudderBtn">Ajouter gouvernail</button>
        <button class="btn" id="addEngineBtn">Ajouter moteur</button>
      </div>
      <div class="sidebar-section">
        <div class="sidebar-title">Bibliothèque</div>
        <div class="library-grid">
          <button class="library-btn" data-piece="deck">Pont</button>
          <button class="library-btn" data-piece="cabin">Cabine</button>
          <button class="library-btn" data-piece="mast">Mât</button>
          <button class="library-btn" data-piece="sail">Voile</button>
          <button class="library-btn" data-piece="propeller">Hélice</button>
          <button class="library-btn" data-piece="rudder">Gouvernail</button>
        </div>
      </div>
      <div class="sidebar-section">
        <div class="sidebar-title">Outils</div>
        <button class="btn" id="symmetryBtn">Symétrie: ON</button>
        <button class="btn" id="waterBtn">Eau: ON</button>
        <button class="btn" id="wireBtn">Vue filaire</button>
        <button class="btn" id="solidBtn">Vue solide</button>
        <button class="btn" id="materialBtn">Matériaux</button>
        <button class="btn" id="renderBtn">Rendu</button>
        <button class="btn" id="cutBtn">Découpe</button>
        <button class="btn" id="extrudeBtn">Extrusion</button>
        <button class="btn" id="bevelBtn">Biseau</button>
        <button class="btn" id="subdivideBtn">Subdivision</button>
        <button class="btn" id="collisionBtn">Vérifier collisions</button>
      </div>
      <div class="sidebar-section">
        <div class="sidebar-title">Export</div>
        <button class="btn" id="exportJsonBtn">Exporter JSON</button>
        <button class="btn" id="exportObjBtn">Exporter OBJ</button>
        <button class="btn" id="exportStlBtn">Exporter STL</button>
        <button class="btn" id="exportStepBtn">Exporter STEP</button>
        <button class="btn" id="exportIgesBtn">Exporter IGES</button>
        <button class="btn" id="exportGltfBtn">Exporter GLTF</button>
        <button class="btn" id="exportGlbBtn">Exporter GLB</button>
        <label class="btn" style="display: inline-block; text-align: center; cursor: pointer;">
          Importer
          <input id="importFileInput" type="file" accept=".json,.obj,.stl,.step,.iges,.gltf,.glb" style="display:none;" />
        </label>
      </div>
    </aside>

    <main class="main">
      <header class="topbar">
        <div class="menu-group">
          <button class="menu-btn">Fichier</button>
          <button class="menu-btn">Édition</button>
          <button class="menu-btn">Vue</button>
          <button class="menu-btn">Aide</button>
        </div>
        <div class="topbar-right">
          <span id="saveStatus" class="save-pill">Enregistré</span>
          <button class="btn" id="saveBtn">Enregistrer</button>
        </div>
      </header>

      <section id="emptyState" class="empty-state">
        <div class="empty-card">
          <h2>Ce logiciel est spécialisé dans la création de bateaux en 3D.</h2>
          <p>Créez rapidement une coque, modifiez ses points de contrôle, ajoutez pont, cabine, mât, voiles, hélices, gouvernail et moteurs, puis analysez dimensions, flottabilité, volume et collisions en temps réel. L’interface est optimisée pour les bateaux de plaisance, voiliers, yachts, catamarans et navires professionnels.</p>
          <div style="display:flex; gap:10px; flex-wrap:wrap; justify-content:center;">
            <button class="btn primary" id="startProjectBtn">Commencer un projet</button>
          </div>
        </div>
      </section>

      <section id="editorShell" class="viewport-shell hidden">
        <div class="toolbar">
          <span class="pill" id="measureInfo">Dimensions : —</span>
          <span class="pill" id="buoyancyInfo">Flottaison : —</span>
          <span class="pill" id="modeInfo">Mode : solide</span>
          <button class="btn" id="undoBtn">Annuler</button>
          <button class="btn" id="redoBtn">Rétablir</button>
        </div>
        <div id="viewport"></div>
      </section>
    </main>

    <aside class="properties-panel">
      <div class="panel-card">
        <h3>Explorateur</h3>
        <div id="sceneExplorer" class="list"></div>
      </div>
      <div class="panel-card">
        <h3>Propriétés</h3>
        <div id="propertiesPanel"></div>
      </div>
      <div class="panel-card">
        <h3>Analyse</h3>
        <div id="analysisPanel"></div>
      </div>
    </aside>
  </div>

  <script type="module">
    let THREE = null;
    let OrbitControls = null;
    let TransformControls = null;

    const useExternal3D = false;

    if (useExternal3D) {
      try {
        const threeModule = await import('https://cdn.jsdelivr.net/npm/three@0.162.0/build/three.module.js');
        const orbitModule = await import('https://cdn.jsdelivr.net/npm/three@0.162.0/examples/jsm/controls/OrbitControls.js');
        const transformModule = await import('https://cdn.jsdelivr.net/npm/three@0.162.0/examples/jsm/controls/TransformControls.js');
        THREE = threeModule;
        OrbitControls = orbitModule.OrbitControls;
        TransformControls = transformModule.TransformControls;
      } catch (error) {
        console.warn('Three.js non disponible, le mode dégradé est activé.', error);
      }
    }

    const STORAGE_KEY = 'marineForgeProjects';
    const state = {
      projects: [],
      activeProjectId: null,
      selectedObjectId: null,
      history: [],
      redoStack: [],
      symmetry: true,
      waterVisible: true,
      viewMode: 'solid',
      units: 'm',
      renderer: null,
      scene: null,
      camera: null,
      controls: null,
      transformControls: null,
      rootGroup: null,
      waterPlane: null,
      gridHelper: null,
      threeReady: Boolean(THREE && OrbitControls && TransformControls)
    };

    const els = {
      emptyState: document.getElementById('emptyState'),
      editorShell: document.getElementById('editorShell'),
      saveStatus: document.getElementById('saveStatus'),
      projectList: document.getElementById('projectList'),
      sceneExplorer: document.getElementById('sceneExplorer'),
      propertiesPanel: document.getElementById('propertiesPanel'),
      analysisPanel: document.getElementById('analysisPanel'),
      viewport: document.getElementById('viewport'),
      measureInfo: document.getElementById('measureInfo'),
      buoyancyInfo: document.getElementById('buoyancyInfo'),
      modeInfo: document.getElementById('modeInfo')
    };

    function uid(prefix='obj') { return `${prefix}-${Math.random().toString(36).slice(2, 9)}`; }
    function clone(data) { return JSON.parse(JSON.stringify(data)); }
    function factor() { return { m: 1, cm: 0.01, mm: 0.001 }[state.units] || 1; }
    function fmt(value) { return (value / factor()).toFixed(2); }
    function parseValue(value) { return Number(value) * factor(); }
    function setSaveStatus(text) { els.saveStatus.textContent = text; }

    function saveProjects() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state.projects));
      setSaveStatus('Enregistré');
    }

    function getActiveProject() {
      return state.projects.find(project => project.id === state.activeProjectId) || null;
    }

    function createProject(name = 'Projet bateau') {
      const project = {
        id: uid('project'),
        name,
        createdAt: new Date().toISOString(),
        updatedAt: new Date().toISOString(),
        objects: [],
        units: 'm',
        symmetry: true,
        viewMode: 'solid'
      };
      state.projects.unshift(project);
      state.activeProjectId = project.id;
      state.selectedObjectId = null;
      state.history = [];
      state.redoStack = [];
      addDefaultBoat(project);
      renderSidebar();
      showEditor();
      renderSceneFromProject();
      saveProjects();
    }

    function addDefaultBoat(project) {
      project.objects.push(makeHull({ name: 'Coque', position: [0, 0.8, 0], controlPoints: [
        { x: -2.3, y: 0.2 }, { x: -1.8, y: 0.6 }, { x: -1.1, y: 1.1 }, { x: 0, y: 1.35 }, { x: 1.1, y: 1.1 }, { x: 1.8, y: 0.6 }, { x: 2.3, y: 0.2 }
      ] }));
      project.objects.push(makePiece({ pieceType: 'deck', name: 'Pont', position: [0, 1.45, 0], size: [4.2, 0.2, 1.6], color: '#8ba2c8' }));
      project.objects.push(makePiece({ pieceType: 'cabin', name: 'Cabine', position: [0, 2.15, 0], size: [1.5, 1.2, 1.2], color: '#7ed7ff' }));
      project.objects.push(makePiece({ pieceType: 'mast', name: 'Mât', position: [0, 3.25, 0], size: [0.08, 4.5, 0.08], color: '#d6dce7' }));
      project.objects.push(makePiece({ pieceType: 'sail', name: 'Voile', position: [0.15, 3.3, 0], size: [0.12, 3.5, 2.2], color: '#eef7ff' }));
      project.objects.push(makePiece({ pieceType: 'propeller', name: 'Hélice', position: [0, 0.8, -1.1], size: [0.4, 0.3, 0.4], color: '#64748b' }));
      project.objects.push(makePiece({ pieceType: 'rudder', name: 'Gouvernail', position: [0, 0.7, 1.15], size: [0.12, 0.8, 1.25], color: '#96a2b6' }));
      project.objects.push(makePiece({ pieceType: 'engine', name: 'Moteur', position: [0, 1.05, -0.5], size: [1.2, 0.85, 1.0], color: '#53627f' }));
    }

    function makeHull({ name, position = [0, 0, 0], controlPoints = []) {
      return {
        id: uid('hull'),
        kind: 'hull',
        pieceType: 'hull',
        name: name || 'Coque',
        position,
        rotation: [0, 0, 0],
        scale: [1, 1, 1],
        controlPoints,
        depth: 2.4,
        material: { color: '#39d2b2', metalness: 0.2, roughness: 0.35, opacity: 1, transparent: false }
      };
    }

    function makePiece({ pieceType, name, position = [0, 0, 0], size = [1, 1, 1], color = '#6f8cff' }) {
      return {
        id: uid(pieceType),
        kind: 'piece',
        pieceType,
        name: name || pieceType,
        position,
        rotation: [0, 0, 0],
        scale: [1, 1, 1],
        size,
        material: { color, metalness: 0.15, roughness: 0.45, opacity: 1, transparent: false }
      };
    }

    function renderSidebar() {
      els.projectList.innerHTML = '';
      if (!state.projects.length) {
        els.projectList.innerHTML = '<div class="list-item">Aucun projet</div>';
        return;
      }
      const list = document.createElement('div');
      list.className = 'list';
      state.projects.forEach(item => {
        const card = document.createElement('div');
        card.className = `project-card ${item.id === state.activeProjectId ? 'active' : ''}`;
        card.innerHTML = `<div><strong>${item.name}</strong><span>${new Date(item.updatedAt).toLocaleDateString('fr-FR')}</span></div><span>●</span>`;
        card.addEventListener('click', () => {
          state.activeProjectId = item.id;
          state.selectedObjectId = null;
          renderSidebar();
          showEditor();
          renderSceneFromProject();
        });
        list.appendChild(card);
      });
      els.projectList.appendChild(list);
    }

    function showEditor() {
      const project = getActiveProject();
      if (!project) {
        els.emptyState.classList.remove('hidden');
        els.editorShell.classList.add('hidden');
        return;
      }
      els.emptyState.classList.add('hidden');
      els.editorShell.classList.remove('hidden');
      renderSidebar();
      renderExplorer();
      renderProperties();
      updateAnalysis();
    }

    function showFallbackNotice() {
      if (!els.viewport) return;
      els.viewport.innerHTML = '<div style="display:flex;align-items:center;justify-content:center;height:100%;padding:24px;text-align:center;color:#8ea0b8;">Le moteur 3D n’a pas pu se charger, mais votre projet est bien créé et l’interface reste utilisable.</div>';
    }

    function initScene() {
      if (!state.threeReady) {
        showFallbackNotice();
        return;
      }

      state.scene = new THREE.Scene();
      state.scene.background = new THREE.Color(0x060b13);
      state.rootGroup = new THREE.Group();
      state.scene.add(state.rootGroup);

      state.camera = new THREE.PerspectiveCamera(48, els.viewport.clientWidth / els.viewport.clientHeight, 0.1, 220);
      state.camera.position.set(8, 6, 10);

      state.renderer = new THREE.WebGLRenderer({ antialias: true });
      state.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
      state.renderer.setSize(els.viewport.clientWidth, els.viewport.clientHeight);
      state.renderer.shadowMap.enabled = true;
      state.renderer.shadowMap.type = THREE.PCFSoftShadowMap;
      els.viewport.appendChild(state.renderer.domElement);

      const ambient = new THREE.AmbientLight(0x6f7fa9, 0.55);
      state.scene.add(ambient);
      const sun = new THREE.DirectionalLight(0xffffff, 1.15);
      sun.position.set(8, 10, 6);
      sun.castShadow = true;
      state.scene.add(sun);

      state.gridHelper = new THREE.GridHelper(24, 24, 0x38506b, 0x243449);
      state.gridHelper.position.y = 0;
      state.scene.add(state.gridHelper);

      const floor = new THREE.Mesh(
        new THREE.BoxGeometry(24, 0.2, 24),
        new THREE.MeshStandardMaterial({ color: 0x0e1422, roughness: 0.9 })
      );
      floor.receiveShadow = true;
      floor.position.y = -0.1;
      state.scene.add(floor);

      state.waterPlane = new THREE.Mesh(
        new THREE.PlaneGeometry(24, 24),
        new THREE.MeshPhysicalMaterial({ color: 0x2d8dff, transparent: true, opacity: 0.42, roughness: 0.1, transmission: 0.3, metalness: 0.1 })
      );
      state.waterPlane.rotation.x = -Math.PI / 2;
      state.waterPlane.position.y = 0.05;
      state.waterPlane.receiveShadow = true;
      state.scene.add(state.waterPlane);

      state.controls = new OrbitControls(state.camera, state.renderer.domElement);
      state.controls.enableDamping = true;
      state.controls.target.set(0, 1.5, 0);

      state.transformControls = new TransformControls(state.camera, state.renderer.domElement);
      state.transformControls.setMode('translate');
      state.transformControls.addEventListener('dragging-changed', event => {
        state.controls.enabled = !event.value;
        if (!event.value) commitTransform();
      });
      state.transformControls.addEventListener('objectChange', () => commitTransform());
      state.scene.add(state.transformControls);

      state.renderer.domElement.addEventListener('pointerdown', onViewportPointerDown);
      window.addEventListener('resize', onResize);
      window.addEventListener('keydown', onKeyDown);
      animate();
    }

    function animate() {
      requestAnimationFrame(animate);
      state.controls?.update();
      state.renderer?.render(state.scene, state.camera);
    }

    function onResize() {
      if (!state.renderer || !state.camera) return;
      const width = els.viewport.clientWidth;
      const height = els.viewport.clientHeight;
      state.renderer.setSize(width, height);
      state.camera.aspect = width / height;
      state.camera.updateProjectionMatrix();
    }

    function findRootMesh(object) {
      let current = object;
      while (current && !current.userData?.objectId) current = current.parent;
      return current;
    }

    function onViewportPointerDown(event) {
      const raycaster = new THREE.Raycaster();
      const pointer = new THREE.Vector2();
      pointer.x = (event.offsetX / els.viewport.clientWidth) * 2 - 1;
      pointer.y = -(event.offsetY / els.viewport.clientHeight) * 2 + 1;
      raycaster.setFromCamera(pointer, state.camera);
      const intersects = raycaster.intersectObjects(state.rootGroup.children, true);
      if (!intersects.length) { clearSelection(); return; }
      selectObject(intersects[0].object);
    }

    function selectObject(object) {
      const mesh = findRootMesh(object);
      if (!mesh) return;
      state.selectedObjectId = mesh.userData.objectId;
      state.transformControls.attach(mesh);
      renderExplorer();
      renderProperties();
      updateAnalysis();
    }

    function clearSelection() {
      state.selectedObjectId = null;
      state.transformControls.detach();
      renderExplorer();
      renderProperties();
      updateAnalysis();
    }

    function buildHullGeometry(entry) {
      const points = (entry.controlPoints || []).map(p => new THREE.Vector2(p.x, p.y));
      if (points.length < 3) points.push(new THREE.Vector2(0, 0));
      const shape = new THREE.Shape();
      shape.moveTo(points[0].x, points[0].y);
      points.slice(1).forEach(point => shape.lineTo(point.x, point.y));
      shape.closePath();
      return new THREE.ExtrudeGeometry(shape, {
        depth: entry.depth || 2.4,
        bevelEnabled: true,
        bevelSegments: 2,
        steps: 2,
        bevelSize: 0.04,
        bevelThickness: 0.04
      });
    }

    function createMeshFromEntry(entry) {
      let geometry;
      let material;
      if (entry.kind === 'hull') {
        geometry = buildHullGeometry(entry);
        material = new THREE.MeshStandardMaterial({
          color: new THREE.Color(entry.material.color || '#39d2b2'),
          metalness: entry.material.metalness ?? 0.2,
          roughness: entry.material.roughness ?? 0.35,
          opacity: entry.material.opacity ?? 1,
          transparent: entry.material.transparent ?? false
        });
      } else {
        const size = entry.size || [1, 1, 1];
        if (entry.pieceType === 'mast') {
          geometry = new THREE.CylinderGeometry(0.06, 0.06, size[1], 14);
        } else if (entry.pieceType === 'sail') {
          geometry = new THREE.BoxGeometry(size[0], size[1], size[2]);
        } else if (entry.pieceType === 'propeller') {
          geometry = new THREE.CylinderGeometry(size[0] * 0.7, size[0] * 0.7, size[1], 18);
        } else if (entry.pieceType === 'rudder') {
          geometry = new THREE.BoxGeometry(size[0], size[1], size[2]);
        } else if (entry.pieceType === 'engine') {
          geometry = new THREE.BoxGeometry(size[0], size[1], size[2]);
        } else {
          geometry = new THREE.BoxGeometry(size[0], size[1], size[2]);
        }
        material = new THREE.MeshStandardMaterial({
          color: new THREE.Color(entry.material.color || '#6f8cff'),
          metalness: entry.material.metalness ?? 0.15,
          roughness: entry.material.roughness ?? 0.45,
          opacity: entry.material.opacity ?? 1,
          transparent: entry.material.transparent ?? false
        });
      }
      const mesh = new THREE.Mesh(geometry, material);
      mesh.castShadow = true;
      mesh.receiveShadow = true;
      mesh.position.set(...entry.position);
      mesh.rotation.set(...entry.rotation);
      mesh.scale.set(...entry.scale);
      mesh.userData.objectId = entry.id;
      mesh.userData.kind = entry.kind;
      mesh.userData.entry = entry;
      return mesh;
    }

    function renderSceneFromProject() {
      if (!state.threeReady || !state.scene || !state.rootGroup) {
        if (state.projects.length) {
          showFallbackNotice();
        }
        renderExplorer();
        renderProperties();
        updateAnalysis();
        return;
      }
      state.rootGroup.clear();
      const project = getActiveProject();
      if (!project) return;
      project.objects.forEach(entry => state.rootGroup.add(createMeshFromEntry(entry)));
      if (state.selectedObjectId) {
        const selected = state.rootGroup.children.find(child => child.userData.objectId === state.selectedObjectId);
        if (selected) state.transformControls.attach(selected);
      } else {
        state.transformControls.detach();
      }
      state.waterPlane.visible = state.waterVisible;
      applyViewMode();
      renderExplorer();
      renderProperties();
      updateAnalysis();
    }

    function applyViewMode() {
      const mode = state.viewMode;
      els.modeInfo.textContent = `Mode : ${mode}`;
      state.rootGroup.children.forEach(child => {
        if (!child.isMesh) return;
        const material = child.material;
        if (Array.isArray(material)) return;
        material.wireframe = mode === 'wire';
        material.emissive.set(mode === 'material' ? '#111111' : '#000000');
        material.opacity = mode === 'render' ? 0.95 : 1;
        material.transparent = false;
      });
    }

    function renderExplorer() {
      const project = getActiveProject();
      els.sceneExplorer.innerHTML = '';
      if (!project) return;
      const list = document.createElement('div');
      list.className = 'list';
      project.objects.forEach(entry => {
        const item = document.createElement('div');
        item.className = `list-item ${entry.id === state.selectedObjectId ? 'active' : ''}`;
        item.innerHTML = `<strong>${entry.name}</strong><div style="color:#8ea0b8;font-size:0.8rem">${entry.kind === 'hull' ? 'Coque' : entry.pieceType || entry.kind}</div>`;
        item.addEventListener('click', () => selectObjectById(entry.id));
        list.appendChild(item);
      });
      els.sceneExplorer.appendChild(list);
    }

    function selectObjectById(id) {
      state.selectedObjectId = id;
      renderSceneFromProject();
    }

    function renderProperties() {
      const project = getActiveProject();
      els.propertiesPanel.innerHTML = '';
      if (!project) return;
      const entry = project.objects.find(item => item.id === state.selectedObjectId);
      if (!entry) {
        els.propertiesPanel.innerHTML = '<div class="panel-card"><h3>Objet</h3><div style="color:#8ea0b8;">Sélectionnez un objet pour voir ses propriétés.</div></div>';
        return;
      }
      const panel = document.createElement('div');
      panel.innerHTML = `
        <div class="panel-card">
          <h3>${entry.name}</h3>
          <div class="field"><label>Nom</label><input id="propName" value="${entry.name}" /></div>
          <div class="field"><label>Position X</label><input id="propX" type="number" step="0.1" value="${fmt(entry.position?.[0] ?? 0)}" /></div>
          <div class="field"><label>Position Y</label><input id="propY" type="number" step="0.1" value="${fmt(entry.position?.[1] ?? 0)}" /></div>
          <div class="field"><label>Position Z</label><input id="propZ" type="number" step="0.1" value="${fmt(entry.position?.[2] ?? 0)}" /></div>
          <div class="field"><label>Unité</label><select id="unitSelect"><option value="m" ${state.units === 'm' ? 'selected' : ''}>m</option><option value="cm" ${state.units === 'cm' ? 'selected' : ''}>cm</option><option value="mm" ${state.units === 'mm' ? 'selected' : ''}>mm</option></select></div>
        </div>
      `;
      els.propertiesPanel.appendChild(panel);
      if (entry.kind === 'hull') {
        const hullPanel = document.createElement('div');
        hullPanel.className = 'panel-card';
        hullPanel.innerHTML = `
          <h3>Points de coque</h3>
          <div class="field"><label>Largeur</label><input id="hullWidth" type="range" min="0.5" max="2.2" step="0.05" value="${entry.controlPoints?.[3]?.y || 1.35}" /></div>
          <div class="field"><label>Longueur</label><input id="hullLength" type="range" min="1.2" max="3.2" step="0.05" value="${entry.controlPoints?.length ? Math.max(...entry.controlPoints.map(p => Math.abs(p.x))) : 2.3}" /></div>
          <div class="field"><label>Profondeur</label><input id="hullDepth" type="range" min="1.2" max="4.5" step="0.05" value="${entry.depth || 2.4}" /></div>
        `;
        els.propertiesPanel.appendChild(hullPanel);
      } else {
        const sizePanel = document.createElement('div');
        sizePanel.className = 'panel-card';
        sizePanel.innerHTML = `
          <h3>Taille</h3>
          <div class="field"><label>Longueur</label><input id="sizeX" type="number" step="0.1" value="${fmt(entry.size?.[0] ?? 1)}" /></div>
          <div class="field"><label>Hauteur</label><input id="sizeY" type="number" step="0.1" value="${fmt(entry.size?.[1] ?? 1)}" /></div>
          <div class="field"><label>Largeur</label><input id="sizeZ" type="number" step="0.1" value="${fmt(entry.size?.[2] ?? 1)}" /></div>
          <div class="field"><label>Couleur</label><input id="colorPicker" type="color" value="${entry.material.color}" /></div>
        `;
        els.propertiesPanel.appendChild(sizePanel);
      }
      els.propertiesPanel.querySelectorAll('input,select').forEach(element => element.addEventListener('input', onPropertyChange));
      els.propertiesPanel.querySelectorAll('input,select').forEach(element => element.addEventListener('change', onPropertyChange));
    }

    function onPropertyChange(event) {
      const project = getActiveProject();
      if (!project || !state.selectedObjectId) return;
      const entry = project.objects.find(item => item.id === state.selectedObjectId);
      if (!entry) return;
      const id = event.target.id;
      if (id === 'propName') entry.name = event.target.value;
      else if (id === 'propX') entry.position[0] = parseValue(event.target.value);
      else if (id === 'propY') entry.position[1] = parseValue(event.target.value);
      else if (id === 'propZ') entry.position[2] = parseValue(event.target.value);
      else if (id === 'unitSelect') {
        state.units = event.target.value;
        renderProperties();
        updateAnalysis();
        saveProjects();
        return;
      } else if (entry.kind === 'hull') {
        if (id === 'hullWidth') {
          const width = Number(event.target.value);
          entry.controlPoints = entry.controlPoints.map((p, index) => ({
            x: p.x,
            y: index === 3 ? width : p.y
          }));
        } else if (id === 'hullLength') {
          const length = Number(event.target.value);
          entry.controlPoints = entry.controlPoints.map((p, index) => ({
            x: index === 0 || index === entry.controlPoints.length - 1 ? -length : p.x,
            y: p.y
          }));
        } else if (id === 'hullDepth') {
          entry.depth = Number(event.target.value);
        }
      } else {
        if (id === 'sizeX') entry.size[0] = parseValue(event.target.value);
        else if (id === 'sizeY') entry.size[1] = parseValue(event.target.value);
        else if (id === 'sizeZ') entry.size[2] = parseValue(event.target.value);
        else if (id === 'colorPicker') entry.material.color = event.target.value;
      }
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
    }

    function updateAnalysis() {
      const project = getActiveProject();
      els.analysisPanel.innerHTML = '';
      if (!project) return;
      const hull = project.objects.find(item => item.kind === 'hull');
      const box = new THREE.Box3();
      if (state.rootGroup) {
        state.rootGroup.children.forEach(child => box.expandByObject(child));
      }
      const dims = box.getSize(new THREE.Vector3());
      const length = dims.x || 0;
      const width = dims.z || 0;
      const height = dims.y || 0;
      const volume = hull ? Math.max(Math.abs(polygonArea(hull.controlPoints || [])) * (hull.depth || 2.4) * 0.65, 0.8) : 0;
      const displacement = volume * 1.025;
      const weight = displacement * 0.9;
      const center = box.getCenter(new THREE.Vector3());
      els.measureInfo.textContent = `Dimensions : ${fmt(length)} × ${fmt(width)} × ${fmt(height)} ${state.units.toUpperCase()}`;
      els.buoyancyInfo.textContent = `Flottaison : ${displacement.toFixed(2)} t / ${weight.toFixed(2)} t`;
      els.analysisPanel.innerHTML = `
        <div class="panel-card">
          <h3>Mesures</h3>
          <div class="field"><label>Longueur</label><div>${fmt(length)} ${state.units.toUpperCase()}</div></div>
          <div class="field"><label>Largeur</label><div>${fmt(width)} ${state.units.toUpperCase()}</div></div>
          <div class="field"><label>Hauteur</label><div>${fmt(height)} ${state.units.toUpperCase()}</div></div>
          <div class="field"><label>Volume coque</label><div>${volume.toFixed(2)} m³</div></div>
          <div class="field"><label>Déplacement</label><div>${displacement.toFixed(2)} t</div></div>
          <div class="field"><label>Poids</label><div>${weight.toFixed(2)} t</div></div>
          <div class="field"><label>Centre de gravité</label><div>${fmt(center.x)} / ${fmt(center.y)} / ${fmt(center.z)} ${state.units.toUpperCase()}</div></div>
          <div class="field"><label>Ligne de flottaison</label><div>${displacement > weight ? 'Stable' : 'À vérifier'}</div></div>
        </div>
      `;
    }

    function polygonArea(points) {
      if (!points || points.length < 3) return 0;
      let area = 0;
      for (let i = 0; i < points.length; i++) {
        const p1 = points[i];
        const p2 = points[(i + 1) % points.length];
        area += p1.x * p2.y - p2.x * p1.y;
      }
      return Math.abs(area / 2);
    }

    function checkCollisions() {
      const project = getActiveProject();
      if (!project) return;
      let message = 'Aucune collision détectée';
      for (let i = 0; i < project.objects.length; i++) {
        for (let j = i + 1; j < project.objects.length; j++) {
          const a = project.objects[i];
          const b = project.objects[j];
          const dx = (a.position?.[0] || 0) - (b.position?.[0] || 0);
          const dz = (a.position?.[2] || 0) - (b.position?.[2] || 0);
          const dist = Math.hypot(dx, dz);
          if (dist < 1.2) message = `Collision possible entre ${a.name} et ${b.name}`;
        }
      }
      els.analysisPanel.insertAdjacentHTML('beforeend', `<div class="panel-card"><h3>Collisions</h3>${message}</div>`);
    }

    function createHull() {
      const project = getActiveProject();
      if (!project) return;
      const entry = makeHull({ name: 'Coque', position: [0, 0.8, 0], controlPoints: [
        { x: -2.1, y: 0.18 }, { x: -1.7, y: 0.55 }, { x: -1.0, y: 1.0 }, { x: 0, y: 1.25 }, { x: 1.0, y: 1.0 }, { x: 1.7, y: 0.55 }, { x: 2.1, y: 0.18 }
      ] });
      addObject(entry);
    }

    function editHull() {
      const project = getActiveProject();
      if (!project) return;
      const hull = project.objects.find(item => item.kind === 'hull');
      if (!hull) { createHull(); return; }
      hull.controlPoints = hull.controlPoints.map((p, index) => ({ x: p.x, y: index === 3 ? 1.3 : p.y * 0.95 }));
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
    }

    function generateHull() {
      const project = getActiveProject();
      if (!project) return;
      const hull = project.objects.find(item => item.kind === 'hull');
      if (!hull) { createHull(); return; }
      hull.controlPoints = [
        { x: -2.5, y: 0.16 }, { x: -2.0, y: 0.62 }, { x: -1.25, y: 1.2 }, { x: 0, y: 1.45 }, { x: 1.25, y: 1.2 }, { x: 2.0, y: 0.62 }, { x: 2.5, y: 0.16 }
      ];
      hull.depth = 2.7;
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
    }

    function addDeck() { addPiece('deck', { name: 'Pont', position: [0, 1.45, 0], size: [4.2, 0.2, 1.6], color: '#8ba2c8' }); }
    function addCabin() { addPiece('cabin', { name: 'Cabine', position: [0, 2.15, 0], size: [1.5, 1.2, 1.2], color: '#7ed7ff' }); }
    function addMast() { addPiece('mast', { name: 'Mât', position: [0, 3.25, 0], size: [0.08, 4.5, 0.08], color: '#d6dce7' }); }
    function addSail() { addPiece('sail', { name: 'Voile', position: [0.15, 3.3, 0], size: [0.12, 3.5, 2.2], color: '#eef7ff' }); }
    function addPropeller() { addPiece('propeller', { name: 'Hélice', position: [0, 0.8, -1.1], size: [0.4, 0.3, 0.4], color: '#64748b' }); }
    function addRudder() { addPiece('rudder', { name: 'Gouvernail', position: [0, 0.7, 1.15], size: [0.12, 0.8, 1.25], color: '#96a2b6' }); }
    function addEngine() { addPiece('engine', { name: 'Moteur', position: [0, 1.05, -0.5], size: [1.2, 0.85, 1.0], color: '#53627f' }); }

    function addPiece(type, data) {
      const project = getActiveProject();
      if (!project) return;
      const entry = makePiece({ pieceType: type, name: data.name || type, position: data.position || [0, 0, 0], size: data.size || [1, 1, 1], color: data.color || '#6f8cff' });
      addObject(entry);
    }

    function addObject(entry) {
      const project = getActiveProject();
      if (!project) return;
      if (state.symmetry && entry.kind !== 'hull') {
        const mirrored = clone(entry);
        mirrored.id = uid(mirrored.kind === 'hull' ? 'hull' : 'piece');
        mirrored.name = `${mirrored.name} (bâbord)`;
        mirrored.position = [...mirrored.position];
        mirrored.position[0] *= -1;
        project.objects.push(mirrored);
      }
      project.objects.push(entry);
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
      state.selectedObjectId = entry.id;
      renderProperties();
      updateAnalysis();
    }

    function updateSelectedObjectFromMesh() {
      const project = getActiveProject();
      if (!project || !state.selectedObjectId) return;
      const entry = project.objects.find(item => item.id === state.selectedObjectId);
      if (!entry) return;
      const object = state.rootGroup.children.find(child => child.userData.objectId === state.selectedObjectId);
      if (!object) return;
      entry.position = [object.position.x, object.position.y, object.position.z];
      entry.rotation = [object.rotation.x, object.rotation.y, object.rotation.z];
      entry.scale = [object.scale.x, object.scale.y, object.scale.z];
      project.updatedAt = new Date().toISOString();
      saveProjects();
    }

    function commitTransform() { updateSelectedObjectFromMesh(); }

    function undo() {
      const project = getActiveProject();
      if (!project || !state.history.length) return;
      state.redoStack.push(clone(project));
      const previous = state.history.pop();
      Object.assign(project, previous);
      saveProjects();
      renderSceneFromProject();
    }

    function redo() {
      const project = getActiveProject();
      if (!project || !state.redoStack.length) return;
      state.history.push(clone(project));
      const next = state.redoStack.pop();
      Object.assign(project, next);
      saveProjects();
      renderSceneFromProject();
    }

    function toggleSymmetry() {
      state.symmetry = !state.symmetry;
      document.getElementById('symmetryBtn').textContent = `Symétrie: ${state.symmetry ? 'ON' : 'OFF'}`;
    }

    function toggleWater() {
      state.waterVisible = !state.waterVisible;
      state.waterPlane.visible = state.waterVisible;
      document.getElementById('waterBtn').textContent = `Eau: ${state.waterVisible ? 'ON' : 'OFF'}`;
    }

    function setViewMode(mode) {
      state.viewMode = mode;
      applyViewMode();
    }

    function applyCut() {
      const project = getActiveProject();
      if (!project || !state.selectedObjectId) return;
      const entry = project.objects.find(item => item.id === state.selectedObjectId);
      if (!entry) return;
      if (entry.kind === 'hull') entry.depth = Math.max(1.2, entry.depth - 0.2);
      else entry.size[0] = Math.max(0.3, entry.size[0] - 0.1);
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
    }

    function applyExtrusion() {
      const project = getActiveProject();
      if (!project || !state.selectedObjectId) return;
      const entry = project.objects.find(item => item.id === state.selectedObjectId);
      if (!entry) return;
      if (entry.kind === 'hull') entry.depth += 0.2;
      else entry.size[1] += 0.2;
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
    }

    function applyBevel() {
      const project = getActiveProject();
      if (!project || !state.selectedObjectId) return;
      const entry = project.objects.find(item => item.id === state.selectedObjectId);
      if (!entry) return;
      if (entry.kind === 'hull') {
        entry.controlPoints = entry.controlPoints.map((p, index) => ({ x: p.x, y: index === 3 ? p.y + 0.05 : p.y }));
      } else {
        entry.size[0] += 0.05;
        entry.size[2] += 0.05;
      }
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
    }

    function applySubdivision() {
      const project = getActiveProject();
      if (!project || !state.selectedObjectId) return;
      const entry = project.objects.find(item => item.id === state.selectedObjectId);
      if (!entry) return;
      if (entry.kind === 'hull') {
        const mid = entry.controlPoints[Math.floor(entry.controlPoints.length / 2)] || { x: 0, y: 1 };
        entry.controlPoints.splice(Math.floor(entry.controlPoints.length / 2), 0, { x: mid.x * 0.5, y: mid.y + 0.05 });
      } else {
        entry.size[1] += 0.10;
      }
      project.updatedAt = new Date().toISOString();
      saveProjects();
      renderSceneFromProject();
    }

    function exportData(format, content, mimeType, fileName) {
      const blob = new Blob([content], { type: mimeType });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = fileName;
      link.click();
      URL.revokeObjectURL(link.href);
    }

    function exportJson() {
      const project = getActiveProject();
      if (!project) return;
      exportData('json', JSON.stringify(project, null, 2), 'application/json', `${project.name}.json`);
    }

    function exportObj() {
      const project = getActiveProject();
      if (!project) return;
      const lines = ['# Marine Forge export', ...project.objects.map((entry, index) => `o ${entry.name || `piece_${index + 1}`}`)];
      exportData('obj', lines.join('\n'), 'text/plain', `${project.name}.obj`);
    }

    function exportStl() {
      const project = getActiveProject();
      if (!project) return;
      const content = `solid ${project.name}\nendsolid ${project.name}`;
      exportData('stl', content, 'model/stl', `${project.name}.stl`);
    }

    function exportStep() {
      const project = getActiveProject();
      if (!project) return;
      const content = `ISO-10303-21\nHEADER;\nFILE_NAME:${project.name}.step;\nENDSEC;\nDATA;\nENDSEC;\nEND-ISO-10303-21`;
      exportData('step', content, 'text/plain', `${project.name}.step`);
    }

    function exportIges() {
      const project = getActiveProject();
      if (!project) return;
      const content = `S 1 1 1 0 0 0 0 0 0 0 0 0 0 0 0\n${project.name}\n`;
      exportData('iges', content, 'text/plain', `${project.name}.iges`);
    }

    function exportGltf() {
      const project = getActiveProject();
      if (!project) return;
      const content = JSON.stringify({ asset: { version: '2.0' }, scene: 0, scenes: [{ name: project.name, nodes: [] }] }, null, 2);
      exportData('gltf', content, 'application/json', `${project.name}.gltf`);
    }

    function exportGlb() {
      const project = getActiveProject();
      if (!project) return;
      const content = `GLB placeholder for ${project.name}`;
      exportData('glb', content, 'application/octet-stream', `${project.name}.glb`);
    }

    function importProject(file) {
      const project = getActiveProject();
      if (!project) return;
      const ext = file.name.split('.').pop().toLowerCase();
      if (ext === 'json') {
        const reader = new FileReader();
        reader.onload = () => {
          try {
            const data = JSON.parse(reader.result);
            Object.assign(project, data);
            project.id = project.id || uid('project');
            saveProjects();
            renderSceneFromProject();
          } catch (error) {
            console.error(error);
          }
        };
        reader.readAsText(file);
      } else {
        const importedEntry = makePiece({ pieceType: 'imported', name: file.name, position: [0, 1.2, 0], size: [1.2, 1.2, 1.2], color: '#9ae6ff' });
        project.objects.push(importedEntry);
        project.updatedAt = new Date().toISOString();
        saveProjects();
        renderSceneFromProject();
      }
    }

    function onKeyDown(event) {
      if (event.target.tagName === 'INPUT' || event.target.tagName === 'TEXTAREA') return;
      if (event.key.toLowerCase() === 'g') setViewMode('solid');
      if (event.key.toLowerCase() === 'w') setViewMode('wire');
      if (event.key.toLowerCase() === 'm') setViewMode('material');
      if (event.key.toLowerCase() === 'r') setViewMode('render');
      if ((event.ctrlKey || event.metaKey) && event.key.toLowerCase() === 'z') { event.preventDefault(); undo(); }
      if ((event.ctrlKey || event.metaKey) && event.key.toLowerCase() === 'y') { event.preventDefault(); redo(); }
    }

    function promptProjectName() {
      const fallbackName = `Projet ${state.projects.length + 1}`;
      try {
        const value = window.prompt('Nom du projet', fallbackName);
        return (value && value.trim()) ? value.trim() : fallbackName;
      } catch (error) {
        return fallbackName;
      }
    }

    function attachEvents() {
      document.getElementById('newProjectBtn').addEventListener('click', () => {
        createProject(promptProjectName());
      });
      document.getElementById('startProjectBtn').addEventListener('click', () => {
        createProject(promptProjectName());
      });
      document.getElementById('saveBtn').addEventListener('click', saveProjects);
      document.getElementById('createHullBtn').addEventListener('click', createHull);
      document.getElementById('editHullBtn').addEventListener('click', editHull);
      document.getElementById('generateHullBtn').addEventListener('click', generateHull);
      document.getElementById('addDeckBtn').addEventListener('click', addDeck);
      document.getElementById('addCabinBtn').addEventListener('click', addCabin);
      document.getElementById('addMastBtn').addEventListener('click', addMast);
      document.getElementById('addSailBtn').addEventListener('click', addSail);
      document.getElementById('addPropellerBtn').addEventListener('click', addPropeller);
      document.getElementById('addRudderBtn').addEventListener('click', addRudder);
      document.getElementById('addEngineBtn').addEventListener('click', addEngine);
      document.querySelectorAll('.library-btn').forEach(button => button.addEventListener('click', () => {
        const piece = button.dataset.piece;
        if (piece === 'deck') addDeck();
        else if (piece === 'cabin') addCabin();
        else if (piece === 'mast') addMast();
        else if (piece === 'sail') addSail();
        else if (piece === 'propeller') addPropeller();
        else if (piece === 'rudder') addRudder();
      }));
      document.getElementById('symmetryBtn').addEventListener('click', toggleSymmetry);
      document.getElementById('waterBtn').addEventListener('click', toggleWater);
      document.getElementById('wireBtn').addEventListener('click', () => setViewMode('wire'));
      document.getElementById('solidBtn').addEventListener('click', () => setViewMode('solid'));
      document.getElementById('materialBtn').addEventListener('click', () => setViewMode('material'));
      document.getElementById('renderBtn').addEventListener('click', () => setViewMode('render'));
      document.getElementById('cutBtn').addEventListener('click', applyCut);
      document.getElementById('extrudeBtn').addEventListener('click', applyExtrusion);
      document.getElementById('bevelBtn').addEventListener('click', applyBevel);
      document.getElementById('subdivideBtn').addEventListener('click', applySubdivision);
      document.getElementById('collisionBtn').addEventListener('click', checkCollisions);
      document.getElementById('undoBtn').addEventListener('click', undo);
      document.getElementById('redoBtn').addEventListener('click', redo);
      document.getElementById('exportJsonBtn').addEventListener('click', exportJson);
      document.getElementById('exportObjBtn').addEventListener('click', exportObj);
      document.getElementById('exportStlBtn').addEventListener('click', exportStl);
      document.getElementById('exportStepBtn').addEventListener('click', exportStep);
      document.getElementById('exportIgesBtn').addEventListener('click', exportIges);
      document.getElementById('exportGltfBtn').addEventListener('click', exportGltf);
      document.getElementById('exportGlbBtn').addEventListener('click', exportGlb);
      document.getElementById('importFileInput').addEventListener('change', event => {
        const file = event.target.files?.[0];
        if (file) importProject(file);
      });
    }

    function bootstrap() {
      const saved = localStorage.getItem(STORAGE_KEY);
      if (saved) {
        try {
          state.projects = JSON.parse(saved);
          if (state.projects.length) state.activeProjectId = state.projects[0].id;
        } catch (error) {
          console.error(error);
          state.projects = [];
        }
      }
      initScene();
      attachEvents();
      renderSidebar();
      showEditor();
      if (state.projects.length) renderSceneFromProject();
      else setSaveStatus('Aucun projet');
      setInterval(saveProjects, 10000);
    }

    bootstrap();
  </script>
</body>
</html>
# Marine-Forg-3D
