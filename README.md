<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#0f0e0d">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>BrewLog — Coffee Journal</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300&family=Noto+Sans+Thai:wght@300;400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.44.0/tabler-icons.min.css">
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#0f0e0d;--bg2:#1a1816;--bg3:#242220;--bg4:#2e2b28;
  --gold:#c9a96e;--gold2:#e8c98a;--cream:#f5f0e8;
  --muted:#6b6560;--border:#3a3530;
  --red:#d4644a;--green:#6aad7a;
  --font-th:'Noto Sans Thai',sans-serif;
  --font-mono:'DM Mono',monospace;
}
body{background:var(--bg);color:var(--cream);font-family:var(--font-th);min-height:100vh;max-width:420px;margin:0 auto;overflow-x:hidden;}
.app{display:flex;flex-direction:column;min-height:100vh;}

/* Nav */
.nav{display:flex;align-items:center;justify-content:space-between;padding:16px 20px 12px;border-bottom:0.5px solid var(--border);}
.nav-logo{font-family:var(--font-mono);font-size:18px;letter-spacing:0.08em;color:var(--gold);}
.nav-logo span{color:var(--cream);opacity:0.5;font-size:11px;display:block;letter-spacing:0.15em;margin-top:-2px;}
.nav-icons{display:flex;gap:16px;}
.nav-icons i{font-size:20px;color:var(--muted);cursor:pointer;transition:color 0.2s;}
.nav-icons i:hover{color:var(--gold);}
.nav-icons i.active{color:var(--gold);}

/* Tabs */
.tabs{display:flex;padding:0 20px;gap:0;border-bottom:0.5px solid var(--border);}
.tab{padding:12px 0;font-size:13px;color:var(--muted);cursor:pointer;margin-right:24px;position:relative;font-family:var(--font-mono);letter-spacing:0.05em;transition:color 0.2s;}
.tab.active{color:var(--gold);}
.tab.active::after{content:'';position:absolute;bottom:-0.5px;left:0;right:0;height:1.5px;background:var(--gold);}

/* Content */
.content{flex:1;padding:20px;overflow-y:auto;}
.page{display:none;}
.page.active{display:block;}

/* Section Headers */
.sec-label{font-family:var(--font-mono);font-size:10px;letter-spacing:0.2em;color:var(--muted);text-transform:uppercase;margin-bottom:12px;margin-top:24px;}
.sec-label:first-child{margin-top:0;}

/* Scan Area */
.scan-zone{background:var(--bg2);border:1.5px dashed var(--border);border-radius:16px;padding:32px 20px;text-align:center;cursor:pointer;transition:all 0.2s;position:relative;overflow:hidden;}
.scan-zone:hover{border-color:var(--gold);background:var(--bg3);}
.scan-zone .scan-icon{font-size:40px;color:var(--gold);opacity:0.7;margin-bottom:12px;}
.scan-zone p{font-size:14px;color:var(--muted);line-height:1.6;}
.scan-zone small{font-family:var(--font-mono);font-size:10px;color:var(--muted);opacity:0.6;display:block;margin-top:8px;}
#camera-input{position:absolute;inset:0;opacity:0;cursor:pointer;width:100%;height:100%;}

/* Detected card */
.detected-card{background:var(--bg2);border:0.5px solid var(--border);border-radius:16px;padding:16px;margin-top:16px;position:relative;}
.detected-card .badge{background:var(--green);color:#0a1a0e;font-size:10px;font-family:var(--font-mono);padding:3px 8px;border-radius:20px;position:absolute;top:16px;right:16px;letter-spacing:0.1em;}
.detected-card h3{font-size:18px;font-weight:600;margin-bottom:4px;color:var(--cream);}
.detected-card .sub{font-size:13px;color:var(--muted);margin-bottom:12px;font-family:var(--font-mono);}
.tag-row{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:12px;}
.tag{background:var(--bg3);border:0.5px solid var(--border);border-radius:20px;padding:4px 10px;font-size:12px;color:var(--gold);font-family:var(--font-mono);}
.info-row{display:flex;justify-content:space-between;font-size:12px;color:var(--muted);}
.info-row span{color:var(--cream);}

/* Form Controls */
.field{margin-bottom:16px;}
.field label{font-size:12px;color:var(--muted);display:block;margin-bottom:6px;font-family:var(--font-mono);letter-spacing:0.08em;}
.field input[type=number],
.field input[type=text],
.field textarea{
  width:100%;background:var(--bg2);border:0.5px solid var(--border);border-radius:10px;
  padding:10px 14px;color:var(--cream);font-size:15px;font-family:var(--font-th);
  outline:none;transition:border-color 0.2s;
}
.field input:focus,.field textarea:focus{border-color:var(--gold);}
.field input[type=number]{font-family:var(--font-mono);}
.unit-field{position:relative;}
.unit-field input{padding-right:40px;}
.unit-field .unit{position:absolute;right:12px;top:50%;transform:translateY(-50%);font-size:12px;color:var(--muted);font-family:var(--font-mono);}

/* Toggle Row */
.toggle-row{display:flex;gap:8px;}
.toggle-btn{flex:1;padding:10px;background:var(--bg2);border:0.5px solid var(--border);border-radius:10px;color:var(--muted);font-size:13px;font-family:var(--font-th);cursor:pointer;text-align:center;transition:all 0.2s;}
.toggle-btn.active{background:var(--bg4);border-color:var(--gold);color:var(--gold);}

/* Grind Visual */
.grind-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;}
.grind-opt{background:var(--bg2);border:1px solid var(--border);border-radius:12px;padding:10px 6px;text-align:center;cursor:pointer;transition:all 0.2s;}
.grind-opt.active{border-color:var(--gold);background:var(--bg3);}
.grind-visual{font-size:24px;margin-bottom:4px;}
.grind-name{font-size:11px;color:var(--muted);font-family:var(--font-mono);}
.grind-micron{font-size:10px;color:var(--gold);font-family:var(--font-mono);margin-top:2px;}

/* Ratio display */
.ratio-badge{background:var(--bg3);border:0.5px solid var(--border);border-radius:10px;padding:12px 16px;display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;}
.ratio-label{font-size:11px;color:var(--muted);font-family:var(--font-mono);}
.ratio-val{font-family:var(--font-mono);font-size:20px;color:var(--gold);letter-spacing:0.05em;}

/* Pour steps */
.pour-step{background:var(--bg2);border:0.5px solid var(--border);border-radius:12px;padding:12px;margin-bottom:8px;}
.pour-step-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:8px;}
.pour-step-num{font-family:var(--font-mono);font-size:11px;color:var(--gold);background:var(--bg3);padding:3px 8px;border-radius:20px;}
.pour-row{display:flex;gap:8px;}
.pour-row .field{flex:1;margin-bottom:0;}

/* Stars */
.star-row{display:flex;gap:8px;justify-content:center;padding:8px 0;}
.star{font-size:32px;cursor:pointer;color:var(--border);transition:all 0.2s;line-height:1;}
.star.lit{color:var(--gold);}
.star:hover{transform:scale(1.2);}
.star-label{text-align:center;font-family:var(--font-mono);font-size:12px;color:var(--muted);margin-top:4px;}

/* Save button */
.save-btn{width:100%;padding:18px;background:var(--gold);color:#0f0e0d;font-size:16px;font-weight:600;font-family:var(--font-th);border:none;border-radius:14px;cursor:pointer;margin-top:20px;transition:all 0.15s;letter-spacing:0.05em;}
.save-btn:hover{background:var(--gold2);}
.save-btn:active{transform:scale(0.98);}

/* History */
.history-item{background:var(--bg2);border:0.5px solid var(--border);border-radius:14px;padding:16px;margin-bottom:12px;cursor:pointer;transition:border-color 0.2s;}
.history-item:hover{border-color:var(--gold);}
.history-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;}
.history-name{font-size:15px;font-weight:500;color:var(--cream);}
.history-date{font-family:var(--font-mono);font-size:11px;color:var(--muted);}
.history-stars{color:var(--gold);font-size:14px;margin-bottom:6px;}
.history-meta{display:flex;gap:8px;flex-wrap:wrap;}
.meta-chip{background:var(--bg3);border-radius:6px;padding:3px 8px;font-size:11px;font-family:var(--font-mono);color:var(--muted);}
.meta-chip span{color:var(--cream);}

/* Detail Modal */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,0.75);z-index:200;display:flex;align-items:flex-end;justify-content:center;}
.modal-sheet{background:var(--bg2);border-radius:20px 20px 0 0;padding:24px 20px 40px;width:100%;max-width:420px;max-height:85vh;overflow-y:auto;}
.modal-handle{width:36px;height:4px;background:var(--border);border-radius:2px;margin:0 auto 20px;}
.modal-title{font-size:18px;font-weight:600;margin-bottom:4px;}
.modal-sub{font-size:13px;color:var(--muted);font-family:var(--font-mono);margin-bottom:16px;}
.modal-row{display:flex;justify-content:space-between;padding:8px 0;border-bottom:0.5px solid var(--border);font-size:13px;}
.modal-row:last-child{border-bottom:none;}
.modal-row .key{color:var(--muted);font-family:var(--font-mono);font-size:12px;}
.modal-row .val{color:var(--cream);text-align:right;max-width:60%;}
.modal-close{width:100%;padding:14px;background:var(--bg3);border:0.5px solid var(--border);border-radius:10px;color:var(--muted);font-family:var(--font-th);font-size:14px;cursor:pointer;margin-top:16px;}
.modal-delete{width:100%;padding:14px;background:transparent;border:0.5px solid var(--red);border-radius:10px;color:var(--red);font-family:var(--font-th);font-size:14px;cursor:pointer;margin-top:8px;}

/* Empty state */
.empty{text-align:center;padding:60px 20px;color:var(--muted);}
.empty i{font-size:48px;opacity:0.3;display:block;margin-bottom:16px;}
.empty p{font-size:14px;line-height:1.7;}

/* Divider */
.divider{height:0.5px;background:var(--border);margin:20px 0;}

/* Cold section */
.cold-section{display:none;}
.cold-section.visible{display:block;}

/* OCR Processing */
.ocr-processing{text-align:center;padding:32px;color:var(--muted);}
.ocr-processing .spinner{display:inline-block;width:32px;height:32px;border:2px solid var(--border);border-top-color:var(--gold);border-radius:50%;animation:spin 0.8s linear infinite;margin-bottom:16px;}
@keyframes spin{to{transform:rotate(360deg)}}

/* Toast */
.toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(20px);background:var(--bg3);border:0.5px solid var(--gold);border-radius:10px;padding:12px 20px;font-size:14px;color:var(--cream);opacity:0;transition:all 0.3s;z-index:1000;white-space:nowrap;font-family:var(--font-mono);}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0);}

/* Scrollbar */
::-webkit-scrollbar{width:4px;}
::-webkit-scrollbar-track{background:transparent;}
::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px;}

.default-badge{background:var(--bg3);border:0.5px solid var(--border);border-radius:6px;padding:2px 8px;font-size:10px;font-family:var(--font-mono);color:var(--muted);margin-left:8px;vertical-align:middle;}

/* API key banner */
.api-banner{background:var(--bg3);border:0.5px solid var(--border);border-radius:10px;padding:10px 14px;margin-bottom:16px;font-size:12px;color:var(--muted);line-height:1.6;}
.api-banner strong{color:var(--gold);}
.api-banner input{width:100%;background:var(--bg2);border:0.5px solid var(--border);border-radius:8px;padding:8px 12px;color:var(--cream);font-size:12px;font-family:var(--font-mono);outline:none;margin-top:6px;}
.api-banner input:focus{border-color:var(--gold);}
</style>
</head>
<body>
<div class="app">
  <div class="nav">
    <div class="nav-logo">
      BREW<br><span>coffee journal</span>
    </div>
    <div class="nav-icons">
      <i class="ti ti-coffee active" id="tab-brew-icon" onclick="setPage('brew')" title="ชง"></i>
      <i class="ti ti-history" id="tab-history-icon" onclick="setPage('history')" title="ประวัติ"></i>
    </div>
  </div>

  <div class="tabs">
    <div class="tab active" id="tab-brew" onclick="setPage('brew')">ชง</div>
    <div class="tab" id="tab-history" onclick="setPage('history')">ประวัติ</div>
  </div>

  <div class="content">
    <!-- BREW PAGE -->
    <div class="page active" id="page-brew">

      <!-- API Key Setup -->
      <div class="api-banner">
        <strong>🔑 Anthropic API Key</strong> (สำหรับสแกนซอง OCR)<br>
        ใส่ key เพื่อเปิดใช้งาน AI สแกนอัตโนมัติ — เก็บใน localStorage เครื่องคุณเท่านั้น
        <input type="password" id="api-key-input" placeholder="sk-ant-api03-..." oninput="saveApiKey(this.value)">
      </div>

      <!-- Scan Section -->
      <div class="sec-label">สแกนซองกาแฟ</div>
      <div class="scan-zone" id="scan-zone">
        <input type="file" accept="image/*" capture="environment" id="camera-input" onchange="handleScan(event)">
        <div class="scan-icon"><i class="ti ti-scan"></i></div>
        <p>แตะเพื่อถ่ายภาพหน้าซองกาแฟ</p>
        <small>AI จะดึงข้อมูล ชื่อ / Origin / Taste Notes ให้อัตโนมัติ</small>
      </div>

      <div id="ocr-processing" style="display:none;" class="ocr-processing">
        <div class="spinner"></div>
        <p style="font-size:13px;">กำลังวิเคราะห์ภาพด้วย AI...</p>
      </div>

      <div id="detected-card" style="display:none;" class="detected-card">
        <span class="badge">✓ สแกนแล้ว</span>
        <h3 id="det-name">—</h3>
        <div class="sub" id="det-origin">—</div>
        <div class="tag-row" id="det-tags"></div>
        <div class="info-row">
          <div>วันคั่ว <span id="det-roast">—</span></div>
          <div id="det-days-since" style="color:var(--green)">—</div>
        </div>
      </div>

      <!-- Manual Entry -->
      <div class="sec-label" style="margin-top:24px;">ข้อมูลกาแฟ <small class="default-badge">กรอกเองได้</small></div>
      <div class="field">
        <label>ชื่อกาแฟ</label>
        <input type="text" id="f-name" placeholder="เช่น Ethiopia Yirgacheffe">
      </div>
      <div class="field">
        <label>Origin / แหล่งที่มา</label>
        <input type="text" id="f-origin" placeholder="เช่น Ethiopia, Yirgacheffe">
      </div>
      <div class="field">
        <label>Taste Notes</label>
        <input type="text" id="f-notes" placeholder="เช่น Berry, Jasmine, Dark Chocolate">
      </div>
      <div class="field">
        <label>วันที่คั่ว</label>
        <input type="text" id="f-roast" placeholder="เช่น 2025-04-20">
      </div>

      <div class="divider"></div>

      <!-- Method -->
      <div class="sec-label">การชง</div>
      <div class="field">
        <label>Method</label>
        <div class="toggle-row">
          <div class="toggle-btn active" style="flex:2;cursor:default;opacity:0.8;">
            <i class="ti ti-coffee" style="font-size:16px;vertical-align:-2px;margin-right:4px;"></i>Aeropress
          </div>
        </div>
      </div>

      <div class="field">
        <label>แบบ</label>
        <div class="toggle-row">
          <div class="toggle-btn active" id="btn-hot" onclick="setBrewType('hot')">☀️ ร้อน</div>
          <div class="toggle-btn" id="btn-cold" onclick="setBrewType('cold')">🧊 เย็น</div>
        </div>
      </div>

      <!-- Dose & Water -->
      <div style="display:flex;gap:10px;">
        <div class="field" style="flex:1;">
          <label>กาแฟ (Dose)</label>
          <div class="unit-field">
            <input type="number" id="f-dose" placeholder="18" oninput="calcRatio()" min="1" max="50" step="0.5">
            <span class="unit">g</span>
          </div>
        </div>
        <div class="field" style="flex:1;">
          <label>น้ำรวม</label>
          <div class="unit-field">
            <input type="number" id="f-water" placeholder="250" oninput="calcRatio()" min="1" max="1000">
            <span class="unit">ml</span>
          </div>
        </div>
      </div>

      <div class="ratio-badge">
        <div>
          <div class="ratio-label">Auto Ratio</div>
          <div class="ratio-val" id="ratio-display">—</div>
        </div>
        <i class="ti ti-calculator" style="font-size:20px;color:var(--muted);"></i>
      </div>

      <!-- Grind Size -->
      <div class="sec-label">ขนาดการบด (Grind Size)</div>
      <div class="grind-grid">
        <div class="grind-opt active" onclick="setGrind(0)" id="grind-0">
          <div class="grind-visual">🌫️</div>
          <div class="grind-name">Fine</div>
          <div class="grind-micron">~150–300µm</div>
        </div>
        <div class="grind-opt" onclick="setGrind(1)" id="grind-1">
          <div class="grind-visual">🧂</div>
          <div class="grind-name">Medium-Fine</div>
          <div class="grind-micron">~300–500µm</div>
        </div>
        <div class="grind-opt" onclick="setGrind(2)" id="grind-2">
          <div class="grind-visual">🫙</div>
          <div class="grind-name">Medium</div>
          <div class="grind-micron">~500–700µm</div>
        </div>
        <div class="grind-opt" onclick="setGrind(3)" id="grind-3">
          <div class="grind-visual">🪨</div>
          <div class="grind-name">Medium-Coarse</div>
          <div class="grind-micron">~700–900µm</div>
        </div>
        <div class="grind-opt" onclick="setGrind(4)" id="grind-4">
          <div class="grind-visual">🏖️</div>
          <div class="grind-name">Coarse</div>
          <div class="grind-micron">~900–1200µm</div>
        </div>
        <div class="grind-opt" onclick="setGrind(5)" id="grind-5">
          <div class="grind-visual">🪵</div>
          <div class="grind-name">Extra Coarse</div>
          <div class="grind-micron">&gt;1200µm</div>
        </div>
      </div>

      <div class="divider"></div>

      <!-- Pour Steps -->
      <div class="sec-label">ลำดับการเท</div>

      <!-- Bloom -->
      <div class="pour-step">
        <div class="pour-step-header">
          <span class="pour-step-num">Bloom</span>
        </div>
        <div class="pour-row">
          <div class="field">
            <label>ปริมาณน้ำ</label>
            <div class="unit-field">
              <input type="number" id="f-bloom-water" placeholder="40" min="0">
              <span class="unit">ml</span>
            </div>
          </div>
          <div class="field">
            <label>เวลา</label>
            <div class="unit-field">
              <input type="number" id="f-bloom-time" placeholder="30" min="0">
              <span class="unit">วิ</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Pour 2 -->
      <div class="pour-step">
        <div class="pour-step-header">
          <span class="pour-step-num">เท 2</span>
        </div>
        <div class="pour-row">
          <div class="field">
            <label>ปริมาณน้ำ</label>
            <div class="unit-field">
              <input type="number" id="f-pour2-water" placeholder="" min="0">
              <span class="unit">ml</span>
            </div>
          </div>
          <div class="field">
            <label>เวลา</label>
            <div class="unit-field">
              <input type="number" id="f-pour2-time" placeholder="" min="0">
              <span class="unit">วิ</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Pour 3 -->
      <div class="pour-step">
        <div class="pour-step-header">
          <span class="pour-step-num">เท 3</span>
        </div>
        <div class="pour-row">
          <div class="field">
            <label>ปริมาณน้ำ</label>
            <div class="unit-field">
              <input type="number" id="f-pour3-water" placeholder="" min="0">
              <span class="unit">ml</span>
            </div>
          </div>
          <div class="field">
            <label>เวลา</label>
            <div class="unit-field">
              <input type="number" id="f-pour3-time" placeholder="" min="0">
              <span class="unit">วิ</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Steep & Press -->
      <div style="display:flex;gap:10px;">
        <div class="field" style="flex:1;">
          <label>เวลาแช่ (Steep)</label>
          <div class="unit-field">
            <input type="number" id="f-steep" placeholder="1" min="0" step="0.5">
            <span class="unit">นาที</span>
          </div>
        </div>
        <div class="field" style="flex:1;">
          <label>เวลา Press</label>
          <div class="unit-field">
            <input type="number" id="f-press" placeholder="30" min="0">
            <span class="unit">วิ</span>
          </div>
        </div>
      </div>

      <!-- Cold section -->
      <div id="cold-section" class="cold-section">
        <div class="sec-label">สำหรับกาแฟเย็น</div>
        <div style="display:flex;gap:10px;">
          <div class="field" style="flex:1;">
            <label>น้ำแข็ง</label>
            <div class="unit-field">
              <input type="number" id="f-ice" placeholder="80" min="0">
              <span class="unit">g</span>
            </div>
          </div>
          <div class="field" style="flex:1;">
            <label>น้ำ Dilute</label>
            <div class="unit-field">
              <input type="number" id="f-dilute" placeholder="30" min="0">
              <span class="unit">ml</span>
            </div>
          </div>
        </div>
      </div>

      <div class="divider"></div>

      <!-- Taste Rating -->
      <div class="sec-label">ประเมินรสชาติ</div>
      <p style="font-size:13px;color:var(--muted);margin-bottom:12px;line-height:1.6;">ได้รสชาติตาม Taste Notes บนซองไหม?</p>
      <div class="star-row" id="star-row">
        <span class="star lit" onclick="setStar(1)">★</span>
        <span class="star lit" onclick="setStar(2)">★</span>
        <span class="star lit" onclick="setStar(3)">★</span>
        <span class="star" onclick="setStar(4)">★</span>
        <span class="star" onclick="setStar(5)">★</span>
      </div>
      <div class="star-label" id="star-label">พอใช้ — บางส่วน</div>

      <div class="field" style="margin-top:16px;">
        <label>โน้ต / ความคิดเห็นเพิ่มเติม</label>
        <textarea id="f-memo" rows="3" style="resize:none;" placeholder="เช่น ต้องบดหยาบขึ้นหน่อย, เพิ่มเวลา steep..."></textarea>
      </div>

      <button class="save-btn" onclick="saveSession()">
        <i class="ti ti-device-floppy" style="font-size:18px;vertical-align:-2px;margin-right:6px;"></i>
        บันทึกทันที
      </button>

      <div style="height:40px;"></div>
    </div>

    <!-- HISTORY PAGE -->
    <div class="page" id="page-history">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;">
        <div class="sec-label" style="margin:0;">บันทึกการชงทั้งหมด</div>
        <div id="history-count" style="font-family:var(--font-mono);font-size:11px;color:var(--muted);"></div>
      </div>
      <div id="history-list">
        <div class="empty">
          <i class="ti ti-coffee-off"></i>
          <p>ยังไม่มีบันทึกการชง<br>ลองชงแก้วแรกเลย!</p>
        </div>
      </div>
      <div style="height:40px;"></div>
    </div>
  </div>
</div>

<!-- Detail Modal -->
<div class="modal-overlay" id="modal-overlay" style="display:none;" onclick="closeModal(event)">
  <div class="modal-sheet" id="modal-sheet">
    <div class="modal-handle"></div>
    <div class="modal-title" id="modal-title">—</div>
    <div class="modal-sub" id="modal-sub">—</div>
    <div id="modal-stars" style="color:var(--gold);font-size:18px;margin-bottom:12px;"></div>
    <div id="modal-body"></div>
    <button class="modal-close" onclick="document.getElementById('modal-overlay').style.display=\"none\"">ปิด</button>
    <button class="modal-delete" id="modal-delete-btn">ลบบันทึกนี้</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const GRIND_LABELS=['Fine','Medium-Fine','Medium','Medium-Coarse','Coarse','Extra Coarse'];
const GRIND_MICRONS=['~150–300µm','~300–500µm','~500–700µm','~700–900µm','~900–1200µm','>1200µm'];
const STAR_LABELS=['','ไม่เลย 😞','พอมีบ้าง 😐','พอใช้ — บางส่วน 🙂','ดีมาก — ใกล้เคียง 😊','เยี่ยม — ตรงมาก ✨'];

let currentGrind = 0;
let currentStar = 3;
let brewType = 'hot';
let currentModalId = null;

// Init API key
window.addEventListener('DOMContentLoaded', () => {
  const savedKey = localStorage.getItem('brewlog_apikey') || '';
  if (savedKey) document.getElementById('api-key-input').value = savedKey;
  applyLastDefaults();
});

function saveApiKey(val) {
  localStorage.setItem('brewlog_apikey', val.trim());
}

function setPage(p) {
  document.querySelectorAll('.page').forEach(x => x.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(x => x.classList.remove('active'));
  document.getElementById('page-' + p).classList.add('active');
  document.getElementById('tab-' + p).classList.add('active');
  document.getElementById('tab-brew-icon').classList.toggle('active', p === 'brew');
  document.getElementById('tab-history-icon').classList.toggle('active', p === 'history');
  if (p === 'history') renderHistory();
}

function setBrewType(t) {
  brewType = t;
  document.getElementById('btn-hot').classList.toggle('active', t === 'hot');
  document.getElementById('btn-cold').classList.toggle('active', t === 'cold');
  const cs = document.getElementById('cold-section');
  if (t === 'cold') cs.classList.add('visible');
  else cs.classList.remove('visible');
}

function setGrind(i) {
  document.querySelectorAll('.grind-opt').forEach((el, j) => el.classList.toggle('active', i === j));
  currentGrind = i;
}

function setStar(n) {
  currentStar = n;
  document.querySelectorAll('.star').forEach((el, i) => {
    el.classList.toggle('lit', i < n);
  });
  document.getElementById('star-label').textContent = STAR_LABELS[n] || '';
}

function calcRatio() {
  const d = parseFloat(document.getElementById('f-dose').value);
  const w = parseFloat(document.getElementById('f-water').value);
  const el = document.getElementById('ratio-display');
  if (d > 0 && w > 0) {
    el.textContent = '1 : ' + (w / d).toFixed(1);
  } else {
    el.textContent = '—';
  }
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2500);
}

async function handleScan(e) {
  const file = e.target.files[0];
  if (!file) return;

  const apiKey = (localStorage.getItem('brewlog_apikey') || '').trim();
  if (!apiKey) {
    showToast('⚠️ ใส่ Anthropic API Key ก่อนสแกน');
    return;
  }

  document.getElementById('ocr-processing').style.display = 'block';
  document.getElementById('detected-card').style.display = 'none';
  document.getElementById('scan-zone').style.display = 'none';

  const reader = new FileReader();
  reader.onload = async (ev) => {
    const base64 = ev.target.result.split(',')[1];
    try {
      const resp = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': apiKey,
          'anthropic-version': '2023-06-01',
          'anthropic-dangerous-direct-browser-access': 'true'
        },
        body: JSON.stringify({
          model: 'claude-sonnet-4-20250514',
          max_tokens: 1000,
          messages: [{
            role: 'user',
            content: [
              { type: 'image', source: { type: 'base64', media_type: file.type, data: base64 } },
              { type: 'text', text: 'Extract coffee bag info from this image. Return ONLY valid JSON (no markdown, no explanation) with these keys: name (string), origin (string), tastingNotes (array of strings, max 5 items), roastDate (string YYYY-MM-DD or "unknown"), variety (string).' }
            ]
          }]
        })
      });
      const data = await resp.json();
      if (data.error) throw new Error(data.error.message);
      const txt = data.content.map(c => c.text || '').join('');
      const clean = txt.replace(/```json|```/g, '').trim();
      const info = JSON.parse(clean);
      populateDetected(info);
    } catch (err) {
      showToast('⚠️ วิเคราะห์ไม่ได้ — กรอกเองได้เลย');
      document.getElementById('ocr-processing').style.display = 'none';
      document.getElementById('scan-zone').style.display = 'block';
    }
  };
  reader.readAsDataURL(file);
}

function populateDetected(info) {
  document.getElementById('ocr-processing').style.display = 'none';
  document.getElementById('detected-card').style.display = 'block';
  document.getElementById('det-name').textContent = info.name || 'Unknown Coffee';
  document.getElementById('det-origin').textContent = info.origin || '';
  const tagsEl = document.getElementById('det-tags');
  tagsEl.innerHTML = '';
  (info.tastingNotes || []).forEach(t => {
    const span = document.createElement('span');
    span.className = 'tag';
    span.textContent = t;
    tagsEl.appendChild(span);
  });
  const rd = info.roastDate && info.roastDate !== 'unknown' ? info.roastDate : '';
  document.getElementById('det-roast').textContent = rd || '—';
  if (rd) {
    const days = Math.floor((Date.now() - new Date(rd)) / 86400000);
    document.getElementById('det-days-since').textContent = days + ' วันหลังคั่ว';
  }
  // Auto-fill form
  document.getElementById('f-name').value = info.name || '';
  document.getElementById('f-origin').value = info.origin || '';
  document.getElementById('f-notes').value = (info.tastingNotes || []).join(', ');
  document.getElementById('f-roast').value = rd;
}

function saveSession() {
  const dose = parseFloat(document.getElementById('f-dose').value) || 0;
  const water = parseFloat(document.getElementById('f-water').value) || 0;
  const name = document.getElementById('f-name').value.trim();
  if (!name) { showToast('กรุณาใส่ชื่อกาแฟ'); return; }
  if (!dose || !water) { showToast('กรุณาใส่ปริมาณกาแฟและน้ำ'); return; }

  const session = {
    id: Date.now(),
    ts: new Date().toISOString(),
    name,
    origin: document.getElementById('f-origin').value.trim(),
    notes: document.getElementById('f-notes').value.trim(),
    roastDate: document.getElementById('f-roast').value.trim(),
    brewType,
    dose, water,
    ratio: (water / dose).toFixed(1),
    grind: currentGrind,
    grindLabel: GRIND_LABELS[currentGrind],
    grindMicron: GRIND_MICRONS[currentGrind],
    bloom: {
      water: document.getElementById('f-bloom-water').value,
      time: document.getElementById('f-bloom-time').value
    },
    pour2: {
      water: document.getElementById('f-pour2-water').value,
      time: document.getElementById('f-pour2-time').value
    },
    pour3: {
      water: document.getElementById('f-pour3-water').value,
      time: document.getElementById('f-pour3-time').value
    },
    steep: document.getElementById('f-steep').value,
    press: document.getElementById('f-press').value,
    ice: brewType === 'cold' ? document.getElementById('f-ice').value : '',
    dilute: brewType === 'cold' ? document.getElementById('f-dilute').value : '',
    star: currentStar,
    memo: document.getElementById('f-memo').value.trim()
  };

  const prev = JSON.parse(localStorage.getItem('brewlog') || '[]');
  prev.unshift(session);
  localStorage.setItem('brewlog', JSON.stringify(prev));
  localStorage.setItem('brewlog_last', JSON.stringify({ dose, water, grind: currentGrind, brewType }));

  showToast('✓ บันทึกแล้ว!');
  setTimeout(() => setPage('history'), 700);
}

function renderHistory() {
  const list = JSON.parse(localStorage.getItem('brewlog') || '[]');
  const el = document.getElementById('history-list');
  const countEl = document.getElementById('history-count');
  if (countEl) countEl.textContent = list.length ? list.length + ' รายการ' : '';

  if (!list.length) {
    el.innerHTML = '<div class="empty"><i class="ti ti-coffee-off"></i><p>ยังไม่มีบันทึกการชง<br>ลองชงแก้วแรกเลย!</p></div>';
    return;
  }

  el.innerHTML = list.map(s => {
    const d = new Date(s.ts);
    const dStr = d.toLocaleDateString('th-TH', { month: 'short', day: 'numeric', hour: '2-digit', minute: '2-digit' });
    const stars = '★'.repeat(s.star) + '☆'.repeat(5 - s.star);
    const tags = (s.notes || '').split(',').filter(Boolean).slice(0, 3)
      .map(t => `<span class="tag" style="font-size:10px;padding:2px 7px;">${t.trim()}</span>`).join('');
    return `<div class="history-item" onclick="showDetail(${s.id})">
      <div class="history-header">
        <div>
          <div class="history-name">${s.name}</div>
          <div style="font-size:12px;color:var(--muted);margin-top:2px;">${s.origin || '—'}</div>
        </div>
        <div class="history-date">${dStr}</div>
      </div>
      <div class="history-stars">${stars}</div>
      <div class="tag-row" style="margin-bottom:8px;">${tags}</div>
      <div class="history-meta">
        <span class="meta-chip">${s.brewType === 'cold' ? '🧊 เย็น' : '☀️ ร้อน'}</span>
        <span class="meta-chip"><span>${s.dose}g</span> / <span>${s.water}ml</span></span>
        <span class="meta-chip">1:<span>${s.ratio}</span></span>
        <span class="meta-chip">${s.grindLabel}</span>
      </div>
    </div>`;
  }).join('');
}

function showDetail(id) {
  const list = JSON.parse(localStorage.getItem('brewlog') || '[]');
  const s = list.find(x => x.id === id);
  if (!s) return;
  currentModalId = id;

  document.getElementById('modal-title').textContent = s.name;
  document.getElementById('modal-sub').textContent = s.origin || '—';
  document.getElementById('modal-stars').textContent = '★'.repeat(s.star) + '☆'.repeat(5 - s.star) + '  ' + STAR_LABELS[s.star];

  const rows = [
    ['วันที่ชง', new Date(s.ts).toLocaleString('th-TH')],
    ['แบบ', s.brewType === 'cold' ? '🧊 เย็น' : '☀️ ร้อน'],
    ['Dose', s.dose + ' g'],
    ['น้ำ', s.water + ' ml'],
    ['Ratio', '1 : ' + s.ratio],
    ['Grind', s.grindLabel + ' (' + s.grindMicron + ')'],
    ['Bloom', s.bloom.water ? s.bloom.water + 'ml / ' + s.bloom.time + 'วิ' : '—'],
    ['เท 2', s.pour2.water ? s.pour2.water + 'ml / ' + s.pour2.time + 'วิ' : '—'],
    ['เท 3', s.pour3.water ? s.pour3.water + 'ml / ' + s.pour3.time + 'วิ' : '—'],
    ['Steep', s.steep ? s.steep + ' นาที' : '—'],
    ['Press', s.press ? s.press + ' วิ' : '—'],
    s.brewType === 'cold' ? ['น้ำแข็ง', s.ice ? s.ice + ' g' : '—'] : null,
    s.brewType === 'cold' ? ['Dilute', s.dilute ? s.dilute + ' ml' : '—'] : null,
    ['Taste Notes', s.notes || '—'],
    s.memo ? ['โน้ต', s.memo] : null,
  ].filter(Boolean);

  document.getElementById('modal-body').innerHTML = rows.map(([k, v]) =>
    `<div class="modal-row"><span class="key">${k}</span><span class="val">${v}</span></div>`
  ).join('');

  document.getElementById('modal-delete-btn').onclick = () => deleteSession(id);
  document.getElementById('modal-overlay').style.display = 'flex';
}

function deleteSession(id) {
  if (!confirm('ลบบันทึกนี้?')) return;
  let list = JSON.parse(localStorage.getItem('brewlog') || '[]');
  list = list.filter(x => x.id !== id);
  localStorage.setItem('brewlog', JSON.stringify(list));
  document.getElementById('modal-overlay').style.display = 'none';
  renderHistory();
  showToast('ลบแล้ว');
}

function closeModal(e) {
  if (e.target === document.getElementById('modal-overlay')) {
    document.getElementById('modal-overlay').style.display = 'none';
  }
}

function applyLastDefaults() {
  const last = JSON.parse(localStorage.getItem('brewlog_last') || 'null');
  if (!last) return;
  if (last.dose) document.getElementById('f-dose').value = last.dose;
  if (last.water) document.getElementById('f-water').value = last.water;
  if (last.grind != null) setGrind(last.grind);
  if (last.brewType) setBrewType(last.brewType);
  calcRatio();
}
</script>
</body>
</html>
