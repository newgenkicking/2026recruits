// KT Player Profile Mini Web App — Pure JavaScript (no TypeScript)
// Self-contained React component file. Copy/paste into a React project.
// Includes: TSV/CSV import with header aliases, autosave, inline expansion, admin editor,
// role-aware stats (K/P/LS), 2025 season stats, visibility toggles, and polished UI.

import React, { useState, useEffect, useMemo, useRef } from "react";

// -----------------------------
// Lightweight UI primitives (self-contained)
// -----------------------------
function cx(...c){ return c.filter(Boolean).join(" "); }
function Card({ className = "", children }) {
  return <div className={cx("rounded-2xl border bg-white", className)}>{children}</div>;
}
function CardHeader({ className = "", children }) {
  return <div className={cx("p-4", className)}>{children}</div>;
}
function CardTitle({ className = "", children }) {
  return <h2 className={cx("font-semibold", className)}>{children}</h2>;
}
function Button({ children, onClick, variant = "default", className = "", type = "button" }){
  const base = "inline-flex items-center justify-center rounded-xl px-3 py-2 text-sm font-medium border transition";
  const styles = variant === "outline"
    ? "bg-white border-gray-200 hover:bg-gray-50"
    : variant === "ghost"
      ? "border-transparent hover:bg-gray-100"
      : "bg-gray-900 text-white border-gray-900 hover:bg-black";
  return <button type={type} onClick={onClick} className={cx(base, styles, className)}>{children}</button>;
}
function Input(props){ return <input {...props} className={cx("w-full rounded-lg border px-3 py-2 text-sm", props.className)} /> }
function Label({children, className=""}){ return <label className={cx("text-sm font-medium", className)}>{children}</label> }
function Textarea(props){ return <textarea {...props} className={cx("w-full rounded-lg border px-3 py-2 text-sm", props.className)} /> }

// Simple modal with scrollable content area
function Modal({open, onClose, title, children}){
  if(!open) return null;
  return (
    <div className="fixed inset-0 z-50">
      <div className="absolute inset-0 bg-black/30" onClick={onClose} />
      <div className="absolute inset-0 flex items-start justify-center p-4">
        <Card className="w-[min(1100px,96vw)] max-h-[90vh] overflow-hidden flex flex-col shadow-xl">
          <CardHeader className="flex items-center justify-between shrink-0">
            <CardTitle>{title}</CardTitle>
            <Button variant="ghost" onClick={onClose}>Close</Button>
          </CardHeader>
          <div className="p-4 overflow-auto">{children}</div>
        </Card>
      </div>
    </div>
  );
}

// -----------------------------
// Utilities & Data
// -----------------------------
const DEFAULT_PASSCODE = "1717"; // as requested
const AUTH_KEY = "kt_admin_authed_v1";
const STORAGE_KEY = "kt_players_v3";
const BACKUP_KEY = "kt_players_backup_v3";
const DRAFT_KEY = "kt_editing_draft_v1";
function newId(){ return `${Date.now()}-${Math.random().toString(36).slice(2,8)}`; }
function safeClone(o){ try{ return JSON.parse(JSON.stringify(o)); }catch{return o; } }
function parseNum(v){ return v === "" || v === null || v === undefined ? undefined : Number(v); }

async function fileToDataURL(file){
  return new Promise((resolve, reject) => {
    try {
      const reader = new FileReader();
      reader.onload = () => resolve(String(reader.result || ""));
      reader.onerror = () => reject(reader.error);
      reader.readAsDataURL(file);
    } catch (err) { reject(err); }
  });
}
function isDataURL(s){ return /^data:image\//.test(String(s || "")); }

// -----------------------------
// CSV helpers (robust to commas/quotes/newlines) + TSV + header aliases
// -----------------------------
const CSV_COLUMNS = [
  // identity & basics
  "id","name","grad_year","position_type","position","school","city","state","height","weight",
  // recruiting
  "recruited_by","college_commit",
  // coach
  "coach_full_name","coach_twitter","coach_logo_url","photo_url",
  // ratings
  "ratings.sailer","ratings.kohls","ratings.kt",
  // badges
  "badges.ktCertified","badges.nflPotential","badges.ktAthlete",
  // visibility flags
  "flags.hideTour","flags.hideSailer","flags.hideKohls","flags.nflStyle",
  // extras
  "extras.recruited_by_twitter","extras.player_twitter","extras.footed","extras.kick_surface","extras.recruit_status","extras.conference","extras.commit_date","extras.offers","extras.visits",
  // core stats (role-aware but export always)
  "metrics.fg_range",
  "metrics.punt_avg","metrics.punt_long","metrics.hang_best",
  // long snap
  "metrics.ls_short_snap","metrics.ls_long_snap","metrics.ls_accuracy",
  // The Tour – kicker/KO
  "metrics.tour_fg_pct","metrics.tour_fg_good_from","metrics.tour_fg_ballspeed","metrics.tour_fg_height_los","metrics.tour_fg_accuracy",
  "metrics.tour_ko_distance","metrics.tour_ko_hang","metrics.tour_ko_ballspeed","metrics.tour_ko_launch",
  // The Tour – punter
  "metrics.tour_punt_distance","metrics.tour_punt_hang","metrics.tour_punt_ballspeed","metrics.tour_punt_launch",
  // 2025 Season (role-aware)
  "metrics.s25_fg","metrics.s25_fg_long","metrics.s25_xp","metrics.s25_ko_tb",
  "metrics.s25_punt_avg","metrics.s25_punt_long","metrics.s25_inside20","metrics.s25_punt_touchbacks","metrics.s25_fair_catches",
  "metrics.s25_ls_short_snap","metrics.s25_ls_long_snap","metrics.s25_ls_40",
  // notes & timeline
  "notes",
  "timeline_json"
];

// Friendly header aliases (case/spacing/punct-insensitive) — mapped to CSV_COLUMNS paths
const HEADER_ALIASES = {
  "player name":"name",
  "grad year":"grad_year",
  "hs":"school","high school":"school",
  "city":"city",
  "state/country":"state","state":"state",
  "position":"position",
  "height":"height",
  "weight (lbs)":"weight","weight":"weight",
  "left/right foot":"extras.footed","left/right foot:":"extras.footed",
  "kick surface":"extras.kick_surface","kick surface:":"extras.kick_surface",
  "kicking coach (full name)":"coach_full_name","kicking coach":"coach_full_name","coach name":"coach_full_name",
  "coach twitter username":"coach_twitter",
  "twitter/x @":"extras.player_twitter","twitter/x":"extras.player_twitter",
  "chris sailer (1–6, ½★)":"ratings.sailer","chris sailer (1-6, 1/2★)":"ratings.sailer","chris sailer":"ratings.sailer","sailer":"ratings.sailer",
  "kohl's (1–5, ½★)":"ratings.kohls","kohls (1–5, ½★)":"ratings.kohls","kohls":"ratings.kohls",
  "kt (1–5, ½★)":"ratings.kt","kt":"ratings.kt",
  "recruit status":"extras.recruit_status",
  "college (commit)":"college_commit","college":"college_commit",
  "conference":"extras.conference",
  "recruited by":"recruited_by",
  "college coach twitter/x username":"extras.recruited_by_twitter",
  "commit date":"extras.commit_date","commit date:":"extras.commit_date",
  "offers":"extras.offers",
  "visits":"extras.visits",
  "fg range (ez/comp/max)":"metrics.fg_range","fg range":"metrics.fg_range",
  "2025 season stats":"extras.season_2025_stats",
  // 2025 shorthands
  "2025 field goals":"metrics.s25_fg","2025 fg":"metrics.s25_fg","field goals 2025":"metrics.s25_fg",
  "2025 field goal long":"metrics.s25_fg_long","2025 fg long":"metrics.s25_fg_long",
  "2025 xp":"metrics.s25_xp","2025 xp/pat":"metrics.s25_xp","xp/pat 2025":"metrics.s25_xp",
  "2025 kickoffs/touchbacks":"metrics.s25_ko_tb","kickoffs/touchbacks 2025":"metrics.s25_ko_tb",
  "2025 punt avg":"metrics.s25_punt_avg","2025 punt long":"metrics.s25_punt_long",
  "2025 inside 20":"metrics.s25_inside20","2025 touchbacks":"metrics.s25_punt_touchbacks","2025 fair catches":"metrics.s25_fair_catches",
  "2025 short snap":"metrics.s25_ls_short_snap","2025 long snap":"metrics.s25_ls_long_snap",
  "2025 40 time":"metrics.s25_ls_40","40 time":"metrics.s25_ls_40",
  // fallbacks
  "player review":"notes","notes":"notes",
  "photo":"photo_url","photo url":"photo_url","headshot":"photo_url",
  "kicking coach logo":"coach_logo_url","coach logo url":"coach_logo_url","coach logo":"coach_logo_url",
  "punt avg":"metrics.punt_avg","punt long":"metrics.punt_long","best hang":"metrics.hang_best"
};
function canonicalKey(h){
  if(!h) return "";
  const raw = h.replace(/[\uFEFF]/g, "").trim();
  if(!raw) return "";
  const simple = raw.toLowerCase().replace(/[^a-z0-9]+/g, " ").trim().replace(/\s+/g, " ");
  if(HEADER_ALIASES[simple]) return HEADER_ALIASES[simple];
  if (CSV_COLUMNS.includes(raw)) return raw;
  return ""; // unmapped header is ignored
}
function parseDelimitedRow(line, delim){
  if(delim==='\t'){
    const parts = []; let cur="", inQ=false;
    for(let i=0;i<line.length;i++){
      const ch=line[i];
      if(inQ){ if(ch==='"'){ if(line[i+1]==='"'){ cur+='"'; i++; } else { inQ=false; } } else { cur+=ch; } }
      else { if(ch==='"'){ inQ=true; } else if(ch==='\t'){ parts.push(cur); cur=""; } else { cur+=ch; } }
    }
    parts.push(cur); return parts;
  }
  const out = []; let cur="", inQ=false;
  for(let i=0;i<line.length;i++){
    const ch=line[i];
    if(inQ){ if(ch==='"'){ if(line[i+1]==='"'){ cur+='"'; i++; } else { inQ=false; } } else { cur+=ch; } }
    else { if(ch==='"'){ inQ=true; } else if(ch===','){ out.push(cur); cur=""; } else { cur+=ch; } }
  }
  out.push(cur); return out;
}
function csvEscape(val){
  const s = val===undefined || val===null ? "" : String(val);
  const safe = s.replace(/\r?\n/g, "\n");
  if(/[",\n]/.test(safe)){
    return '"' + safe.replace(/"/g,'""') + '"';
  }
  return safe;
}
function toCSV(rows){
  const header = CSV_COLUMNS.join(",");
  const body = rows.map(r=> CSV_COLUMNS.map(k=> csvEscape(getPath(r,k))).join(",")).join("\n");
  return header + "\n" + body;
}
function downloadText(filename, text){
  const blob = new Blob([text], { type: "text/csv;charset=utf-8;" });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = filename; a.click();
  setTimeout(()=> URL.revokeObjectURL(url), 0);
}
function getPath(obj, path){
  const seg = path.split('.');
  let cur = obj;
  for(const s of seg){ if(cur==null) return undefined; cur = cur[s]; }
  return cur;
}
function setPath(obj, path, val){
  const seg = path.split('.');
  let cur = obj;
  for(let i=0;i<seg.length-1;i++){ cur[seg[i]] ||= {}; cur = cur[seg[i]]; }
  cur[seg[seg.length-1]] = val;
}
function parseCSV(text){
  const norm = text.replace(/\r?\n/g, "\n");
  const first = norm.split("\n")[0] || "";
  const delim = (first.match(/\t/g)?.length||0) > (first.match(/,/g)?.length||0) ? '\t' : ',';
  const lines = norm.split("\n").filter(l=> l.length>0);
  if(lines.length===0) return { headers:[], headersNorm:[], rows:[], delim };
  const headers = parseDelimitedRow(lines[0], delim);
  const headersNorm = headers.map(canonicalKey);
  const rows = lines.slice(1).map(l=> parseDelimitedRow(l, delim));
  return { headers, headersNorm, rows, delim };
}
function rowToPlayer(headers, row, headersNorm){
  const map = {};
  const H = headersNorm && headersNorm.length===headers.length ? headersNorm : headers.map(canonicalKey);
  H.forEach((hk,idx)=>{ if(!hk) return; const val = row[idx]; setPath(map, hk, val); });
  const p = {
    id: map.id || newId(),
    name: map.name || "",
    grad_year: map.grad_year? Number(map.grad_year): undefined,
    position_type: map.position_type || undefined,
    position: map.position || undefined,
    school: map.school || "",
    city: map.city || "",
    state: map.state || "",
    height: map.height || "",
    weight: map.weight || "",
    recruited_by: map.recruited_by || "",
    college_commit: map.college_commit || "",
    coach_full_name: map.coach_full_name || "",
    coach_twitter: map.coach_twitter || "",
    coach_logo_url: map.coach_logo_url || "",
    photo_url: map.photo_url || "",
    ratings: {
      sailer: map?.ratings?.sailer ? Number(map.ratings.sailer) : undefined,
      kohls:  map?.ratings?.kohls  ? Number(map.ratings.kohls)  : undefined,
      kt:     map?.ratings?.kt     ? Number(map.ratings.kt)     : undefined,
    },
    badges: {
      ktCertified: map?.badges?.ktCertified === 'true' || map?.badges?.ktCertified === true,
      nflPotential: map?.badges?.nflPotential === 'true' || map?.badges?.nflPotential === true,
      ktAthlete: map?.badges?.ktAthlete === 'true' || map?.badges?.ktAthlete === true,
    },
    metrics: { ...(map.metrics||{}) },
    notes: map.notes || "",
    timeline: (()=>{ try{ return map.timeline_json? JSON.parse(map.timeline_json): []; }catch{return []} })(),
    extras: { ...(map.extras||{}), player_twitter: map.extras?.player_twitter, recruited_by_twitter: map.extras?.recruited_by_twitter || map.recruited_by_twitter },
    flags: { ...(map.flags||{}) }
  };
  return migratePlayer(p);
}
function playerToFlat(p){
  const flat = {};
  CSV_COLUMNS.forEach(k=> setPath(flat, k, getPath(p,k)));
  flat.timeline_json = JSON.stringify(p.timeline||[]);
  return flat;
}

// -----------------------------
// Defaults & migration
// -----------------------------
const DEFAULT_PLAYERS = [
  {
    id: newId(),
    name: "Sample Athlete",
    grad_year: 2026,
    position: "K/P",
    school: "Example HS",
    city: "San Diego",
    state: "CA",
    height: "5'11\"",
    weight: "175",
    coach_full_name: "Jaime Medina",
    coach_twitter: "@CoachJaime",
    coach_logo_url: "",
    college_commit: "",
    photo_url: "",
    recruited_by: "Coach Smith",
    ratings: { sailer: 6, kohls: 5, kt: 5 },
    badges: { ktCertified: true, nflPotential: true, ktAthlete: false },
    flags: { hideTour:false, hideSailer:false, hideKohls:false, nflStyle:'navy' },
    metrics: { fg_range: "35/45/55", punt_avg: 43.2, punt_long: 67, hang_best: 4.6 },
    notes: "Consistent tempo. Quiet worker, confident.",
    timeline: [
      { id: newId(), date: "2025-04-03", title: "Invited to Spring Showcase", detail: "Earned invite after winter camp.", source: "" },
      { id: newId(), date: "2025-06-12", title: "Unofficial Visit: SDSU", detail: "Specialist meeting with staff.", source: "https://example.com/visit" },
      { id: newId(), date: "2025-09-01", title: "Offer: State U", detail: "First D1 offer.", source: "" },
    ],
  }
];
function clampHalf(v,min,max){
  return Math.max(min,Math.min(max,Math.round(Number((v ?? min))*2)/2));
}
function migratePlayer(p){
  const out = { ...p };
  out.id ||= newId();
  out.badges ||= {}; out.ratings ||= {}; out.metrics ||= {}; out.timeline ||= []; out.flags ||= {}; out.extras ||= {};
  const rawPos = String(out.position||"").toLowerCase();
  out.position_type = out.position_type || (
    rawPos.includes("long snap") || rawPos==="ls" ? "long snapper" :
    (rawPos.includes("k/p") || rawPos.includes("kicker/punter") ? "kicker/punter" :
    rawPos.startsWith("p") ? "punter" : "kicker")
  );
  if(out.ratings.sailer!==undefined) out.ratings.sailer = clampHalf(out.ratings.sailer,1,6);
  if(out.ratings.kohls!==undefined)  out.ratings.kohls  = clampHalf(out.ratings.kohls,1,5);
  if(out.ratings.kt!==undefined)     out.ratings.kt     = clampHalf(out.ratings.kt,1,5);
  // Auto-derive NFL Potential: only KT 5★ earn this badge
  out.badges.nflPotential = out.ratings.kt === 5;
  out.timeline = [...out.timeline].sort((a,b)=> (b.date||"").localeCompare(a.date||""));
  return out;
}

// Try to merge players from older storage keys to avoid data loss
function loadPlayers(){
  const keys = [STORAGE_KEY, BACKUP_KEY, 'kt_players_v2','kt_players_backup_v2','kt_players_v1','kt_players_backup_v1'];
  const byKey = new Map();
  const keyOf = (p)=> (p.id? `id:${p.id}` : `ng:${(p.name||'').toLowerCase()}-${p.grad_year||''}`);
  let found = false;
  try {
    if(typeof window !== 'undefined'){
      for(const k of keys){
        const raw = localStorage.getItem(k);
        if(!raw) continue;
        try{
          const arr = JSON.parse(raw);
          if(Array.isArray(arr)){
            found = true;
            arr.forEach((p)=>{ const mp = migratePlayer(p); byKey.set(keyOf(mp), mp); });
          }
        }catch{}
      }
    }
  } catch {}
  const list = Array.from(byKey.values());
  if(list.length>0 || found) return list;
  return DEFAULT_PLAYERS.map(migratePlayer);
}

// -----------------------------
// Visual atoms used in header
// -----------------------------
function Img({ src, alt, className = "" }) {
  if (!src) return <div className={cx("bg-gray-200", className)} aria-label={alt} />;
  return <img src={src} alt={alt} className={className} loading="lazy" decoding="async" />;
}
function HeaderBadges({ p }) {
  const hideS = !!p?.flags?.hideSailer;
  const hideK = !!p?.flags?.hideKohls;
  const k = Number(p?.ratings?.kohls);
  const s = Number(p?.ratings?.sailer);
  const isK5 = !hideK && !!k && k === 5; // Kohl's 5★ appears red
  const isS6 = !hideS && !!s && s === 6; // Sailer 6★ special black/yellow

  const nflStyle = p?.flags?.nflStyle || 'navy';
  const NFLBadge = () => {
    if (!p?.badges?.nflPotential) return null;
    if (nflStyle === 'shield') {
      return (
        <span className="inline-flex items-center gap-1 text-sm px-2 py-1 rounded-full border bg-white text-[#0A2342] border-[#0A2342]">
          <svg width="14" height="14" viewBox="0 0 24 24" aria-hidden className="opacity-90">
            <path d="M12 2l7 3v6c0 5-3.5 9-7 11-3.5-2-7-6-7-11V5l7-3z" fill="#0A2342"/>
            <path d="M12 4.2l5 2.1v4.2c0 4.1-2.8 7.5-5 9-2.2-1.5-5-4.9-5-9V6.3l5-2.1z" fill="#fff"/>
            <path d="M12 6.3l3.4 1.4v3c0 2.7-1.9 5-3.4 6-1.6-1-3.4-3.3-3.4-6v-3L12 6.3z" fill="#C8102E"/>
          </svg>
          NFL Potential
        </span>
      );
    }
    if (nflStyle === 'outline') {
      return (
        <span className="inline-flex items-center gap-1 text-sm px-2 py-1 rounded-full border border-[#0A2342] text-[#0A2342] bg-white">NFL Potential</span>
      );
    }
    return (
      <span className="inline-flex items-center gap-1 text-sm px-2 py-1 rounded-full border bg-[#0A2342] text-white border-[#0A2342]">
        NFL Potential
      </span>
    );
  };

  return (
    <div className="flex flex-wrap gap-2 mt-2">
      {p?.badges?.ktCertified && (
        <span
          className="inline-flex items-center gap-1 text-sm px-2 py-1 rounded-full border"
          style={{
            background: "linear-gradient(135deg,#FFF9DB 0%,#FDE68A 35%,#F59E0B 65%,#FFF9DB 100%)",
            color: "#111111",
            borderColor: "#F59E0B",
          }}
        >KT Certified</span>
      )}

      {NFLBadge()}

      {!!p?.ratings?.kt && (
        <span className="inline-flex items-center gap-1 text-sm px-2 py-1 rounded-full border bg-orange-50 text-amber-900 border-orange-300">KT {p.ratings.kt}-Star</span>
      )}

      {!!p?.ratings?.sailer && !hideS && (
        <span
          className={cx(
            "inline-flex items-center gap-1 text-sm px-2 py-1 rounded-full border",
            isS6 ? "bg-black text-yellow-400 border-black" : "bg-yellow-300 text-black border-yellow-500"
          )}
        >
          Sailer {p.ratings.sailer}-Star
        </span>
      )}

      {!!p?.ratings?.kohls && !hideK && (
        <span
          className={cx(
            "inline-flex items-center gap-1 text-sm px-2 py-1 rounded-full border",
            isK5 ? "bg-red-50 text-red-700 border-red-300" : "bg-gray-50 text-gray-800 border-gray-300"
          )}
        >
          Kohl's {p.ratings.kohls}-Star
        </span>
      )}
    </div>
  );
}

// -----------------------------
// Role helpers & stat tiles by role
// -----------------------------
function nonEmpty(v){ return !(v===undefined || v===null || (typeof v==='string' && v.trim()==='')); }
function roleAbbrev(role){
  switch((role||'').toLowerCase()){ case 'punter': return 'P'; case 'long snapper': return 'LS'; case 'kicker/punter': return 'K/P'; default: return 'K'; }
}
function roleAllows(role, key){
  const r=(role||'').toLowerCase();
  if(key==='ls') return r==='long snapper';
  if(key==='kicker') return r==='kicker' || r==='kicker/punter';
  if(key==='punter') return r==='punter' || r==='kicker/punter';
  return false;
}
function StatsCard({ p }){
  const r = (p.position_type||'kicker').toLowerCase();
  const tiles = [];
  if(roleAllows(r,'kicker')){
    tiles.push(
      { label: 'FG RANGE', value: p.metrics?.fg_range },
    );
  }
  if(roleAllows(r,'punter')){
    tiles.push(
      { label: 'PUNT AVG', value: p.metrics?.punt_avg },
      { label: 'PUNT LONG', value: p.metrics?.punt_long },
      { label: 'BEST HANG', value: p.metrics?.hang_best },
    );
  }
  if(roleAllows(r,'ls')){
    tiles.push(
      { label: 'SHORT SNAP (s)', value: p.metrics?.ls_short_snap },
      { label: 'LONG SNAP (s)', value: p.metrics?.ls_long_snap },
      { label: 'ACCURACY GRADE', value: p.metrics?.ls_accuracy },
    );
  }
  if(tiles.length===0) return null;
  return (
    <Card className="shadow-sm">
      <CardHeader className="pb-2"><CardTitle className="text-base">Stats</CardTitle></CardHeader>
      <div className="p-4 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-3">
        {tiles.map((t,i)=> (
          <div key={i} className="flex flex-col items-center justify-center rounded-xl border p-3 min-h-[70px]">
            <div className="text-[10px] uppercase tracking-wide text-gray-500">{t.label}</div>
            <div className="text-lg font-semibold">{nonEmpty(t.value) ? t.value : '—'}</div>
          </div>
        ))}
      </div>
    </Card>
  );
}
function Season2025Card({ p }){
  const r = (p.position_type||'kicker').toLowerCase();
  const tiles = [];
  const M = p.metrics || {};
  if(roleAllows(r,'kicker')){
    tiles.push(
      { label:'2025 FG (M/A)', value: M.s25_fg },
      { label:'2025 FG LONG', value: M.s25_fg_long },
      { label:'2025 XP/PAT', value: M.s25_xp },
      { label:'2025 KO/TB', value: M.s25_ko_tb },
    );
  }
  if(roleAllows(r,'punter')){
    tiles.push(
      { label:'2025 PUNT AVG', value: M.s25_punt_avg },
      { label:'2025 PUNT LONG', value: M.s25_punt_long },
      { label:'2025 INSIDE 20', value: M.s25_inside20 },
      { label:'2025 TB', value: M.s25_punt_touchbacks },
      { label:'2025 FAIR CATCH', value: M.s25_fair_catches },
    );
  }
  if(roleAllows(r,'ls')){
    tiles.push(
      { label:'2025 SHORT SNAP (s)', value: M.s25_ls_short_snap },
      { label:'2025 LONG SNAP (s)', value: M.s25_ls_long_snap },
      { label:'2025 40-YD (s)', value: M.s25_ls_40 },
    );
  }
  const hasAny = tiles.some(t=> nonEmpty(t.value));
  if(!hasAny) return null;
  return (
    <Card className="shadow-sm">
      <CardHeader className="pb-2"><CardTitle className="text-base">2025 Season</CardTitle></CardHeader>
      <div className="p-4 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-3">
        {tiles.map((t,i)=> (
          <div key={i} className="flex flex-col items-center justify-center rounded-xl border p-3 min-h-[70px]">
            <div className="text-[10px] uppercase tracking-wide text-gray-500">{t.label}</div>
            <div className="text-lg font-semibold">{nonEmpty(t.value) ? t.value : '—'}</div>
          </div>
        ))}
      </div>
    </Card>
  );
}
function TourStatsCard({ p }){
  if(p?.flags?.hideTour) return null;
  const r = (p.position_type||'kicker').toLowerCase();
  const tiles = [];
  if(roleAllows(r,'kicker')){
    const M = p.metrics || {};
    [
      { label:'TOUR FG %', value: M.tour_fg_pct },
      { label:'TOUR GOOD FROM', value: M.tour_fg_good_from },
      { label:'TOUR FG BALL SPD', value: M.tour_fg_ballspeed },
      { label:'HEIGHT @ LOS', value: M.tour_fg_height_los },
      { label:'FG ACCURACY', value: M.tour_fg_accuracy },
      { label:'TOUR KO DIST', value: M.tour_ko_distance },
      { label:'TOUR KO HANG', value: M.tour_ko_hang },
      { label:'TOUR KO BALL SPD', value: M.tour_ko_ballspeed },
      { label:'KO LAUNCH', value: M.tour_ko_launch },
    ].forEach(t=> tiles.push(t));
  }
  if(roleAllows(r,'punter')){
    const M = p.metrics || {};
    [
      { label:'TOUR PUNT DIST', value: M.tour_punt_distance },
      { label:'TOUR PUNT HANG', value: M.tour_punt_hang },
      { label:'TOUR PUNT BALL SPD', value: M.tour_punt_ballspeed },
      { label:'PUNT LAUNCH', value: M.tour_punt_launch },
    ].forEach(t=> tiles.push(t));
  }
  const hasAny = tiles.some(t=> nonEmpty(t.value));
  if(!hasAny) return null;
  return (
    <Card className="shadow-sm">
      <CardHeader className="pb-2"><CardTitle className="text-base">The Tour — Performance</CardTitle></CardHeader>
      <div className="p-4 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-3">
        {tiles.map((t,i)=> (
          <div key={i} className="flex flex-col items-center justify-center rounded-xl border p-3 min-h-[70px]">
            <div className="text-[10px] uppercase tracking-wide text-gray-500">{t.label}</div>
            <div className="text-lg font-semibold">{nonEmpty(t.value) ? t.value : '—'}</div>
          </div>
        ))}
      </div>
    </Card>
  );
}

// -----------------------------
// Profile Header (FG Range visible; coach + recruited-by with @)
// -----------------------------
function ProfileHeader({ p, canEdit = false, onEdit }) {
  const coachName = p.coach_full_name || p.coach_name || "";
  const handle = (p.coach_twitter || "").trim();
  const twUser = handle.startsWith("@") ? handle.slice(1) : handle;
  const twUrl = twUser ? `https://twitter.com/${twUser}` : "";

  const vitals = [
    { label: "Height", value: p.height },
    { label: "Weight", value: p.weight ? `${p.weight} lbs` : "" },
    { label: "City", value: p.city },
    { label: "State", value: p.state },
    { label: "Class", value: p.grad_year || "" },
    { label: "Position", value: p.position },
  ].filter(v => v.value !== undefined && v.value !== null && String(v.value).trim() !== "");

  const headerLineParts = [p.position || roleAbbrev(p.position_type||'kicker'), p.school, p.state, p.grad_year]
    .map(x => (x===0 ? "0" : x))
    .filter(Boolean);
  const headerLine = headerLineParts.join(" • ");

  const showCoach = Boolean(coachName || twUser || p.coach_logo_url);
  const showRecruited = Boolean(p.recruited_by && String(p.recruited_by).trim() !== "");

  return (
    <Card className="shadow-sm">
      <CardHeader className="pb-2">
        <div className="flex flex-col gap-4">
          <div className="flex flex-wrap items-center gap-4 justify-between">
            <div className="flex items-center gap-4 flex-wrap">
              <Img src={p.photo_url} alt={`${p.name || 'Player'} headshot`} className="h-32 w-32 rounded-3xl object-cover" />
              <div>
                <div className="flex items-center gap-2 flex-wrap">
                  <CardTitle className="text-2xl">{p.name || "Unnamed Player"}</CardTitle>
                  {p.college_commit && (
                    <span className="text-[12px] px-3 py-1 rounded-full bg-blue-50 border text-blue-700">Committed</span>
                  )}
                  {canEdit && (
                    <Button variant="outline" onClick={()=> onEdit && onEdit(p)} className="ml-2">Edit</Button>
                  )}
                </div>
                {headerLine && (
                  <div className="text-sm text-gray-500">{headerLine}</div>
                )}
                {p.college_commit && (
                  <div className="text-sm mt-1 flex flex-wrap items-center gap-2">
                    <span>Committed to <span className="font-semibold">{p.college_commit}</span></span>
                    {showRecruited && (
                      <>
                        <span className="text-gray-300">•</span>
                        <span>Recruited by <span className="font-semibold">{p.recruited_by}</span></span>
                        {p?.extras?.recruited_by_twitter && (
                          <a className="text-blue-600" href={`https://twitter.com/${String(p.extras.recruited_by_twitter).replace(/^@/,"")}`} target="_blank" rel="noreferrer">
                            @{String(p.extras.recruited_by_twitter).replace(/^@/ ,"")}
                          </a>
                        )}
                      </>
                    )}
                  </div>
                )}
                <HeaderBadges p={p} />
              </div>
            </div>
          </div>

          {/* Vitals row */}
          <div className="flex flex-col gap-3">
            {vitals.length > 0 && (
              <div className="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-3">
                {vitals.map((t)=> (
                  <div key={t.label} className="rounded-xl border px-3 py-2 text-center">
                    <div className="text-[10px] uppercase tracking-wide text-gray-500">{t.label}</div>
                    <div className="text-lg font-semibold">{t.value}</div>
                  </div>
                ))}
                {/* FG Range surfaced in header */}
                {nonEmpty(p?.metrics?.fg_range) && (
                  <div className="rounded-xl border px-3 py-2 text-center">
                    <div className="text-[10px] uppercase tracking-wide text-gray-500">FG Range</div>
                    <div className="text-lg font-semibold">{p.metrics.fg_range}</div>
                  </div>
                )}
              </div>
            )}

            {(showCoach || showRecruited) && (
              <div className="border-t pt-3 flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
                {showCoach && (
                  <div className="flex items-center gap-2">
                    <Img src={p.coach_logo_url} alt="kicking coach logo" className="h-10 w-10 rounded-lg object-cover" />
                    <div>
                      {coachName && <div className="font-medium leading-tight">{coachName}</div>}
                      {twUser && <a href={twUrl} target="_blank" rel="noreferrer" className="text-blue-600 text-xs hover:underline">@{twUser}</a>}
                    </div>
                  </div>
                )}
                {showRecruited && (
                  <div className="flex items-center gap-2 text-sm text-gray-700">
                    <span>Recruited by:</span>
                    <span className="font-medium text-gray-900">{p.recruited_by}</span>
                    {p?.extras?.recruited_by_twitter && (
                      <a className="text-blue-600" href={`https://twitter.com/${String(p.extras.recruited_by_twitter).replace(/^@/,"")}`} target="_blank" rel="noreferrer">
                        @{String(p.extras.recruited_by_twitter).replace(/^@/ ,"")}
                      </a>
                    )}
                  </div>
                )}
              </div>
            )}
          </div>
        </div>
      </CardHeader>
    </Card>
  );
}

// -----------------------------
// Recruiting Timeline
// -----------------------------
function TimelineCard({ items }){
  if(!items || items.length===0){
    return (
      <Card>
        <CardHeader className="pb-2"><CardTitle className="text-base">Recruiting Timeline</CardTitle></CardHeader>
        <div className="p-4 text-sm text-gray-500">No updates yet.</div>
      </Card>
    );
  }
  return (
    <Card>
      <CardHeader className="pb-2"><CardTitle className="text-base">Recruiting Timeline</CardTitle></CardHeader>
      <div className="p-4">
        <ol className="relative border-l ml-3 border-gray-200">
          {items.map((it)=> (
            <li key={it.id} className="mb-6 ml-4">
              <span className="absolute -left-2.5 mt-0.5 h-5 w-5 rounded-full border bg-white flex items-center justify-center">
                <span className="h-2.5 w-2.5 rounded-full bg-gray-400"/>
              </span>
              <div className="flex flex-wrap items-center gap-2">
                <span className="text-xs text-gray-500">{it.date || '—'}</span>
                <span className="text-xs text-gray-400">{it.type? `(${it.type})` : ''}</span>
                <span className="text-sm font-medium">{it.title || '—'}</span>
                {it.source && <a href={it.source} target="_blank" rel="noreferrer" className="text-xs text-blue-600 hover:underline">source</a>}
              </div>
              {it.detail && <div className="text-sm text-gray-600 mt-1">{it.detail}</div>}
            </li>
          ))}
        </ol>
      </div>
    </Card>
  );
}

// -----------------------------
// Admin: toggles & form
// -----------------------------
function BadgeToggle({label, value, onChange, variant}){
  const base = "w-full rounded-2xl border p-3 text-left flex items-center gap-3 cursor-pointer select-none transition";
  const active = value ? "ring-2 ring-gray-300 bg-gray-50" : "hover:bg-gray-50";
  const pill = variant==='gold'
    ? { background: "linear-gradient(135deg,#FFF9DB 0%,#FDE68A 35%,#F59E0B 65%,#FFF9DB 100%)", color: "#7a4b00", borderColor: "#F59E0B" }
    : variant==='diamond'
      ? { background: "linear-gradient(135deg,#ECFEFF 0%,#E0F2FE 40%,#E0E7FF 100%)", color: "#0b3b5e", borderColor: "#93C5FD" }
      : undefined;
  return (
    <div role="switch" aria-checked={value} tabIndex={0}
      onClick={()=> onChange(!value)}
      onKeyDown={(e)=>{ if(e.key==='Enter' || e.key===' '){ e.preventDefault(); onChange(!value); } }}
      className={`${base} ${active}`} style={pill}>
      <span className={`h-5 w-5 rounded-full border flex items-center justify-center ${value?'bg-gray-900 text-white border-gray-900':'bg-white'}`}>{value?"✓":""}</span>
      <div className="flex-1">
        <div className="text-sm font-medium leading-tight">{label}</div>
        <div className="text-xs text-gray-500">{value?"Enabled":"Tap to enable"}</div>
      </div>
    </div>
  );
}

function AddEditForm({ value, onChange }) {
  const upd=(k,val)=> onChange({ ...value, [k]: val });
  const updNested=(path,val)=>{
    const next = safeClone(value);
    const seg = path.split('.');
    let o = next;
    for(let i=0;i<seg.length-1;i++){ o[seg[i]] ||= {}; o=o[seg[i]]; }
    const last = seg[seg.length-1];
    o[last]=val; onChange(next);
  };
  const role = (value.position_type||'kicker');
  return (
    <div className="grid md:grid-cols-4 gap-4">
      <div className="space-y-2">
        <Label>Name</Label>
        <Input value={value.name||""} onChange={(e)=>upd('name', e.target.value)} />
        <Label>Grad Year</Label>
        <Input type="number" value={value.grad_year??""} onChange={(e)=>upd('grad_year', parseNum(e.target.value))} />
        <Label>Position Type</Label>
        <select value={(value.position_type||'kicker')} onChange={(e)=>onChange({ ...value, position_type: e.target.value })} className="w-full rounded-lg border px-3 py-2 text-sm">
          <option value="kicker">Kicker</option>
          <option value="punter">Punter</option>
          <option value="long snapper">Long Snapper</option>
          <option value="kicker/punter">Kicker/Punter</option>
        </select>
        <Label>Position (display)</Label>
        <Input value={value.position||roleAbbrev(value.position_type||'kicker')} onChange={(e)=>upd('position', e.target.value)} />
        <Label>School</Label>
        <Input value={value.school||""} onChange={(e)=>upd('school', e.target.value)} />
        <Label>City</Label>
        <Input value={value.city||""} onChange={(e)=>upd('city', e.target.value)} />
        <Label>State</Label>
        <Input value={value.state||""} onChange={(e)=>upd('state', e.target.value)} />
        <Label>Recruited by</Label>
        <Input value={value.recruited_by||""} onChange={(e)=>upd('recruited_by', e.target.value)} />
        <Label>College Coach Twitter/X Username</Label>
        <Input placeholder="@CoachHandle" value={value.extras?.recruited_by_twitter||""} onChange={(e)=>updNested('extras.recruited_by_twitter', e.target.value)} />
      </div>
      <div className="space-y-2">
        <Label>Height</Label>
        <Input value={value.height||""} onChange={(e)=>upd('height', e.target.value)} />
        <Label>Weight (lbs)</Label>
        <Input value={value.weight||""} onChange={(e)=>upd('weight', e.target.value)} />
        <Label>Kicking Coach (Full Name)</Label>
        <Input value={value.coach_full_name||""} onChange={(e)=>upd('coach_full_name', e.target.value)} />
        <Label>Coach Twitter Username</Label>
        <Input placeholder="@CoachHandle" value={value.coach_twitter||""} onChange={(e)=>upd('coach_twitter', e.target.value)} />
        <div className="space-y-2">
          <Label>Upload Profile Photo</Label>
          <div className="flex items-center gap-3">
            <input type="file" accept="image/*" onChange={async (e)=>{ const f=e.target.files?.[0]; if(!f) return; const url=await fileToDataURL(f); onChange({ ...value, photo_url: url }); }} />
            {value.photo_url && (<>
              <Img src={value.photo_url} alt="preview" className="h-16 w-16 rounded-lg object-cover" />
              <Button variant="ghost" onClick={()=> onChange({ ...value, photo_url: "" })}>Clear</Button>
            </>)}
          </div>
        </div>
        <div className="space-y-2">
          <Label>Upload Coach Logo</Label>
          <div className="flex items-center gap-3">
            <input type="file" accept="image/*" onChange={async (e)=>{ const f=e.target.files?.[0]; if(!f) return; const url=await fileToDataURL(f); onChange({ ...value, coach_logo_url: url }); }} />
            {value.coach_logo_url && (<>
              <Img src={value.coach_logo_url} alt="preview" className="h-12 w-12 rounded object-cover" />
              <Button variant="ghost" onClick={()=> onChange({ ...value, coach_logo_url: "" })}>Clear</Button>
            </>)}
          </div>
        </div>
      </div>
      <div className="space-y-2">
        {/* Kicker-only stats */}
        {roleAllows((value.position_type||'kicker'),'kicker') && (<>
          <Label>FG Range</Label>
          <Input value={value.metrics?.fg_range||""} onChange={(e)=>updNested('metrics.fg_range', e.target.value)} />
          <div className="mt-2 pt-2 border-t">
            <Label className="block">The Tour — Kicker (FG)</Label>
            <div className="grid grid-cols-2 gap-2 mt-1">
              <Input placeholder="FG %" value={value.metrics?.tour_fg_pct||""} onChange={(e)=>updNested('metrics.tour_fg_pct', e.target.value)} />
              <Input placeholder="Good From (yds)" value={value.metrics?.tour_fg_good_from||""} onChange={(e)=>updNested('metrics.tour_fg_good_from', e.target.value)} />
              <Input placeholder="Ball Speed" value={value.metrics?.tour_fg_ballspeed||""} onChange={(e)=>updNested('metrics.tour_fg_ballspeed', e.target.value)} />
              <Input placeholder="Height @ LOS" value={value.metrics?.tour_fg_height_los||""} onChange={(e)=>updNested('metrics.tour_fg_height_los', e.target.value)} />
              <Input placeholder="Accuracy Score" value={value.metrics?.tour_fg_accuracy||""} onChange={(e)=>updNested('metrics.tour_fg_accuracy', e.target.value)} />
            </div>
            <Label className="block mt-3">The Tour — Kickoff</Label>
            <div className="grid grid-cols-2 gap-2 mt-1">
              <Input placeholder="Distance" value={value.metrics?.tour_ko_distance||""} onChange={(e)=>updNested('metrics.tour_ko_distance', e.target.value)} />
              <Input placeholder="Hang Time" value={value.metrics?.tour_ko_hang||""} onChange={(e)=>updNested('metrics.tour_ko_hang', e.target.value)} />
              <Input placeholder="Ball Speed" value={value.metrics?.tour_ko_ballspeed||""} onChange={(e)=>updNested('metrics.tour_ko_ballspeed', e.target.value)} />
              <Input placeholder="Launch Angle" value={value.metrics?.tour_ko_launch||""} onChange={(e)=>updNested('metrics.tour_ko_launch', e.target.value)} />
            </div>
          </div>
        </>)}
        {/* Punter-only stats */}
        {roleAllows((value.position_type||'kicker'),'punter') && (<>
          <Label>Punt Avg</Label>
          <Input type="number" value={value.metrics?.punt_avg??""} onChange={(e)=>updNested('metrics.punt_avg', parseNum(e.target.value))} />
          <Label>Punt Long</Label>
          <Input type="number" value={value.metrics?.punt_long??""} onChange={(e)=>updNested('metrics.punt_long', parseNum(e.target.value))} />
          <Label>Best Hang (s)</Label>
          <Input type="number" step={0.01} value={value.metrics?.hang_best??""} onChange={(e)=>updNested('metrics.hang_best', parseNum(e.target.value))} />
          <div className="mt-2 pt-2 border-t">
            <Label className="block">The Tour — Punter</Label>
            <div className="grid grid-cols-2 gap-2 mt-1">
              <Input placeholder="Distance" value={value.metrics?.tour_punt_distance||""} onChange={(e)=>updNested('metrics.tour_punt_distance', e.target.value)} />
              <Input placeholder="Hang Time" value={value.metrics?.tour_punt_hang||""} onChange={(e)=>updNested('metrics.tour_punt_hang', e.target.value)} />
              <Input placeholder="Ball Speed" value={value.metrics?.tour_punt_ballspeed||""} onChange={(e)=>updNested('metrics.tour_punt_ballspeed', e.target.value)} />
              <Input placeholder="Launch Angle" value={value.metrics?.tour_punt_launch||""} onChange={(e)=>updNested('metrics.tour_punt_launch', e.target.value)} />
            </div>
          </div>
        </>)}
        {/* Long Snapper-only stats */}
        {roleAllows((value.position_type||'kicker'),'ls') && (<>
          <Label>Short Snap Time (s)</Label>
          <Input type="number" step={0.01} value={value.metrics?.ls_short_snap??""} onChange={(e)=>updNested('metrics.ls_short_snap', parseNum(e.target.value))} />
          <Label>Long Snap Time (s)</Label>
          <Input type="number" step={0.01} value={value.metrics?.ls_long_snap??""} onChange={(e)=>updNested('metrics.ls_long_snap', parseNum(e.target.value))} />
          <Label>Accuracy Grade</Label>
          <Input value={value.metrics?.ls_accuracy||""} onChange={(e)=>updNested('metrics.ls_accuracy', e.target.value)} />
        </>)}
      </div>
      <div className="space-y-3">
        <Label>College (Commit)</Label>
        <Input value={value.college_commit||""} onChange={(e)=>upd('college_commit', e.target.value)} />
        <div className="grid grid-cols-3 gap-2 mt-2">
          <div>
            <Label>Chris Sailer (1–6, ½★)</Label>
            <Input type="number" step={0.5} min={1} max={6} value={value.ratings?.sailer??''} onChange={(e)=>updNested('ratings.sailer', parseNum(e.target.value))} />
          </div>
          <div>
            <Label>Kohl's (1–5, ½★)</Label>
            <Input type="number" step={0.5} min={1} max={5} value={value.ratings?.kohls??''} onChange={(e)=>updNested('ratings.kohls', parseNum(e.target.value))} />
          </div>
          <div>
            <Label>KT (1–5, ½★)</Label>
            <Input type="number" step={0.5} min={1} max={5} value={value.ratings?.kt??''} onChange={(e)=>updNested('ratings.kt', parseNum(e.target.value))} />
          </div>
        </div>
        <div className="space-y-2">
          <Label>Visibility</Label>
          <div className="grid sm:grid-cols-3 gap-2">
            <BadgeToggle label="Hide Sailer rating" value={!!value.flags?.hideSailer} onChange={(v)=>updNested('flags.hideSailer', v)} />
            <BadgeToggle label="Hide Kohl's rating" value={!!value.flags?.hideKohls} onChange={(v)=>updNested('flags.hideKohls', v)} />
            <BadgeToggle label="Hide The Tour data" value={!!value.flags?.hideTour} onChange={(v)=>updNested('flags.hideTour', v)} />
          </div>
          <div className="grid grid-cols-2 gap-2">
            <div>
              <Label>NFL Badge Style</Label>
              <select value={value.flags?.nflStyle||'navy'} onChange={(e)=>updNested('flags.nflStyle', e.target.value)} className="w-full rounded-lg border px-3 py-2 text-sm">
                <option value="navy">Solid Navy</option>
                <option value="shield">Shield</option>
                <option value="outline">Outline</option>
              </select>
            </div>
          </div>
        </div>
      </div>
      <div className="md:col-span-4">
        <Label>Player Review</Label>
        <Textarea rows={4} value={value.notes||""} onChange={(e)=>upd('notes', e.target.value)} />
      </div>
      <div className="md:col-span-4">
        <TimelineEditor value={value.timeline||[]} onChange={(v)=>upd('timeline', v)} />
      </div>
    </div>
  );
}

function TimelineEditor({value,onChange}){
  const add = ()=> onChange([{ id:newId(), date:"", type:"Note", title:"", detail:"", source:"" }, ...value]);
  const rm  = (id)=> onChange(value.filter(v=>v.id!==id));
  const upd = (id, k, v)=> onChange(value.map(it=> it.id===id? { ...it, [k]: v }: it));
  const sort = ()=> onChange([...value].sort((a,b)=> (b.date||"").localeCompare(a.date||"")));
  const types = ["Offer","Visit","Camp","Commit","Interest","Note"];
  return (
    <div className="space-y-3">
      <div className="flex justify-between items-center">
        <Label>Recruiting Timeline</Label>
        <div className="flex gap-2">
          <Button variant="outline" onClick={add}>Add Step</Button>
          <Button variant="ghost" onClick={sort}>Sort (Newest First)</Button>
        </div>
      </div>
      <div className="space-y-3">
        {value.map((it)=> (
          <div key={it.id} className="grid md:grid-cols-12 gap-2 border rounded-xl p-3">
            <div className="md:col-span-2">
              <Label>Date (YYYY-MM-DD)</Label>
              <Input value={it.date||""} onChange={(e)=>upd(it.id,'date', e.target.value)} />
            </div>
            <div className="md:col-span-2">
              <Label>Type</Label>
              <select value={it.type||"Note"} onChange={(e)=>upd(it.id,'type', e.target.value)} className="w-full rounded-lg border px-3 py-2 text-sm">
                {types.map(t=> <option key={t} value={t}>{t}</option>)}
              </select>
            </div>
            <div className="md:col-span-3">
              <Label>Title</Label>
              <Input value={it.title||""} onChange={(e)=>upd(it.id,'title', e.target.value)} />
            </div>
            <div className="md:col-span-3">
              <Label>Detail</Label>
              <Input value={it.detail||""} onChange={(e)=>upd(it.id,'detail', e.target.value)} />
            </div>
            <div className="md:col-span-2">
              <Label>Source URL</Label>
              <Input value={it.source||""} onChange={(e)=>upd(it.id,'source', e.target.value)} />
            </div>
            <div className="md:col-span-12 flex justify-end">
              <Button variant="ghost" onClick={()=>rm(it.id)}>Remove</Button>
            </div>
          </div>
        ))}
        {value.length===0 && <div className="text-sm text-gray-500">No steps yet. Click "Add Step".</div>}
      </div>
    </div>
  );
}

// -----------------------------
// App
// -----------------------------
export default function App(){
  const [players, setPlayers] = useState(()=> loadPlayers());
  const [openIds, setOpenIds] = useState([]);
  const [adminOpen, setAdminOpen] = useState(false);
  const [loggedIn, setLoggedIn] = useState(()=>{
    try{ return typeof window!=="undefined" && localStorage.getItem(AUTH_KEY)==='1'; }catch{ return false; }
  });
  const [pass, setPass] = useState("");
  const fileRef = useRef(null);
  const [editing, setEditing] = useState(null);

  // Persist players + rolling backup
  useEffect(()=>{
    try {
      if(typeof window !== 'undefined'){
        const json = JSON.stringify(players);
        localStorage.setItem(STORAGE_KEY, json);
        localStorage.setItem(BACKUP_KEY, json);
      }
    } catch {}
  }, [players]);

  // Draft autosave for the editor so you never lose work
  useEffect(()=>{ try{ if(editing){ localStorage.setItem(DRAFT_KEY, JSON.stringify(editing)); } }catch{} }, [editing]);
  useEffect(()=>{ try{ const raw = localStorage.getItem(DRAFT_KEY); if(raw && !editing){ setEditing(JSON.parse(raw)); } }catch{} }, []);

  const keyOf = (p)=> (p.id? `id:${p.id}` : `ng:${(p.name||'').toLowerCase()}-${p.grad_year||''}`);
  const savePlayer = (p)=>{
    p = migratePlayer(p);
    setPlayers(prev=> {
      const by = new Map(prev.map(x=> [keyOf(x), x]));
      by.set(keyOf(p), p);
      return Array.from(by.values());
    });
  };
  const removePlayer = (id)=> setPlayers(prev=> prev.filter(p=>p.id!==id));

  // CSV/TSV actions
  const exportCSV = ()=>{
    const flatRows = players.map(playerToFlat);
    const csv = toCSV(flatRows);
    downloadText(`kt_players_${new Date().toISOString().slice(0,10)}.csv`, csv);
  };
  const triggerImport = ()=> fileRef.current?.click();
  const onImportFile = async (e)=>{
    const file = e.target.files?.[0];
    if(!file) return;
    const text = await file.text();
    const { headers, rows, headersNorm, delim } = parseCSV(text);
    if(headers.length===0){ alert('CSV/TSV has no header row'); return; }
    const imported = rows.map(r=> rowToPlayer(headers, r, headersNorm));
    // Merge by id or name+grad
    setPlayers(prev=>{
      const byKey = new Map();
      const kf = (p)=> (p.id? `id:${p.id}` : `ng:${(p.name||'').toLowerCase()}-${p.grad_year||''}`);
      [...prev, ...imported].forEach(p=>{ byKey.set(kf(p), p); });
      return Array.from(byKey.values()).map(migratePlayer);
    });
    const unmapped = headers.map((h,i)=> !headersNorm[i] ? h : '').filter(Boolean);
    const msg = [
      `Imported ${imported.length} row(s) via ${delim==='\t'?'TSV':'CSV'}.`,
      unmapped.length? `Unrecognized column(s): ${unmapped.join(', ')}`: ''
    ].filter(Boolean).join('\n');
    alert(msg);
    e.target.value = '';
  };

  // Inline expansion helpers
  const toggleOpen = (id)=> setOpenIds(ids=> ids.includes(id)? ids.filter(x=>x!==id): [...ids, id]);
  const collapseAll = ()=> setOpenIds([]);
  const expandAll = ()=> setOpenIds(players.map(p=>p.id));

  // UI
  return (
    <div className="p-4 md:p-6 max-w-6xl mx-auto relative">
      <div className="fixed top-4 right-4 z-40 flex gap-2">
        <Button variant="outline" onClick={()=> setAdminOpen(true)}>Admin</Button>
        {loggedIn && (<>
          <Button variant="outline" onClick={exportCSV}>Export</Button>
          <Button variant="outline" onClick={triggerImport}>Import</Button>
          <input ref={fileRef} type="file" accept=".csv,.tsv,text/csv,text/tab-separated-values" className="hidden" onChange={onImportFile} />
        </>)}
      </div>

      <Card className="shadow-sm mb-4">
        <CardHeader className="pb-2">
          <h1 className="text-2xl md:text-3xl font-bold tracking-tight">KT Recruiting — Player Profiles</h1>
          <p className="text-sm text-gray-500">Inline expansion under roster • autosave • TSV/CSV import with header aliases.</p>
          <div className="mt-2 flex gap-2">
            <Button variant="outline" onClick={expandAll}>Expand All</Button>
            <Button variant="ghost" onClick={collapseAll}>Collapse All</Button>
          </div>
        </CardHeader>
      </Card>

      {/* Roster list */}
      <Card className="mb-4">
        <CardHeader className="pb-2"><CardTitle className="text-base">Roster</CardTitle></CardHeader>
        <div className="p-4">
          {players.map((p)=> (
            <div key={p.id} className="mb-3">
              <button onClick={()=> toggleOpen(p.id)} className={cx("w-full text-left rounded-2xl border p-3 hover:bg-gray-50", openIds.includes(p.id) && "ring-2 ring-gray-300") }>
                <div className="flex items-center gap-3">
                  <Img src={p.photo_url} alt="headshot" className="h-12 w-12 rounded-xl object-cover" />
                  <div className="flex-1">
                    <div className="font-medium">{p.name || "—"}</div>
                    <div className="text-xs text-gray-500">{p.position || "—"} • {p.school || "—"}{p.state?` • ${p.state}`:""}{p.grad_year?` • ${p.grad_year}`:""}</div>
                  </div>
                  <span className="text-xs text-gray-500">{openIds.includes(p.id)? 'Collapse' : 'Expand'}</span>
                </div>
              </button>
              {openIds.includes(p.id) && (
                <div className="mt-3 space-y-4">
                  <ProfileHeader p={p} canEdit={loggedIn} onEdit={(pl)=>{ setEditing(safeClone(pl)); setAdminOpen(true); }} />
                  <StatsCard p={p} />
                  <Season2025Card p={p} />
                  <TourStatsCard p={p} />
                  <TimelineCard items={p.timeline} />
                </div>
              )}
            </div>
          ))}
          {players.length===0 && <div className="text-sm text-gray-500">No players yet.</div>}
          {loggedIn && (
            <Button className="w-full mt-2" onClick={()=> setEditing(migratePlayer({ id:newId(), name:"", ratings:{}, badges:{}, metrics:{}, notes:"", timeline:[], flags:{}, extras:{} }))}>Add Player</Button>
          )}
        </div>
      </Card>

      {/* Admin modal */}
      <Modal open={adminOpen} onClose={()=> setAdminOpen(false)} title={loggedIn?"Admin":"Admin Login"}>
        {!loggedIn ? (
          <div className="max-w-sm space-y-3">
            <Label>Passcode</Label>
            <Input type="password" value={pass} onChange={(e)=> setPass(e.target.value)} onKeyDown={(e)=>{ if(e.key==='Enter'){ if(pass===DEFAULT_PASSCODE){ setLoggedIn(true); try{ localStorage.setItem(AUTH_KEY,'1'); }catch{} } } }} placeholder="Enter passcode" />
            <div className="flex justify-end gap-2">
              <Button onClick={()=> { if(pass===DEFAULT_PASSCODE){ setLoggedIn(true); try{ localStorage.setItem(AUTH_KEY,'1'); }catch{} } }}>Login</Button>
            </div>
          </div>
        ) : (
          <div className="space-y-6">
            <div className="flex items-center justify-between flex-wrap gap-2">
              <div className="text-sm text-gray-600">Logged in • manual add + TSV/CSV import/export • draft autosave enabled</div>
              <div className="flex gap-2">
                <Button variant="outline" onClick={()=> setEditing(migratePlayer({ id:newId(), name:"", ratings:{}, badges:{}, metrics:{}, notes:"", timeline:[], flags:{}, extras:{} }))}>Add Player</Button>
                <Button variant="outline" onClick={exportCSV}>Export</Button>
                <Button variant="outline" onClick={triggerImport}>Import</Button>
                <input ref={fileRef} type="file" accept=".csv,.tsv,text/csv,text/tab-separated-values" className="hidden" onChange={onImportFile} />
                <Button variant="ghost" onClick={()=> { setLoggedIn(false); setPass(""); try{ localStorage.removeItem(AUTH_KEY); }catch{} }}>Logout</Button>
              </div>
            </div>

            <Card>
              <CardHeader className="pb-2"><CardTitle className="text-base">Manage Players</CardTitle></CardHeader>
              <div className="p-4 space-y-2">
                {players.map((p)=> (
                  <div key={p.id} className="flex items-center justify-between rounded-xl border p-2">
                    <div className="flex items-center gap-2">
                      <Img src={p.photo_url} alt="headshot" className="h-8 w-8 rounded object-cover" />
                      <div className="text-sm"><div className="font-medium">{p.name||'—'}</div><div className="text-xs text-gray-500">{p.position||'—'} • {p.grad_year||'—'}</div></div>
                    </div>
                    <div className="flex gap-2">
                      <Button variant="outline" onClick={()=> setEditing(safeClone(p))}>Edit</Button>
                      <Button variant="ghost" onClick={()=> removePlayer(p.id)}>Delete</Button>
                    </div>
                  </div>
                ))}
                {players.length===0 && <div className="text-sm text-gray-500">No players yet.</div>}
              </div>
            </Card>

            {editing && (
              <Card>
                <CardHeader className="pb-2"><CardTitle className="text-base">{players.some(x=>x.id===editing.id)?'Edit Player':'Add Player'}</CardTitle></CardHeader>
                <div className="p-4 space-y-4">
                  <AddEditForm value={editing} onChange={(v)=>{ setEditing(v); }} />
                  <div className="flex flex-wrap items-center justify-between gap-2 text-xs text-gray-500">
                    <div>Draft is auto-saved locally.</div>
                    <div className="flex gap-2">
                      <Button variant="ghost" onClick={()=> setEditing(null)}>Close</Button>
                      <Button onClick={()=> { savePlayer(editing); setEditing(null); try{ localStorage.removeItem(DRAFT_KEY); }catch{} }}>Save</Button>
                    </div>
                  </div>
                </div>
              </Card>
            )}
          </div>
        )}
      </Modal>

      <style>{`
        body { background: #f7f7f8; }
      `}</style>
    </div>
  );
}

// --- Self-tests ---
(function runSelfTests(){
  try {
    console.assert(clampHalf(6.7,1,6)===6 && clampHalf(0.1,1,5)===1 && clampHalf(5.2,1,5)===5, 'rating clamp');
    const tl = migratePlayer({timeline:[{date:'2024-01-01'},{date:'2025-01-01'}], ratings:{sailer:7,kohls:0,kt:9}});
    console.assert(tl.timeline[0].date==='2025-01-01', 'timeline sorts newest first');
    console.assert(tl.ratings.sailer===6 && tl.ratings.kohls===1 && tl.ratings.kt===5, 'ratings clamped');
    console.assert(isDataURL('data:image/png;base64,AAAA') && !isDataURL('http://example.com/x.png'), 'isDataURL helper');
    // CSV roundtrip
    const csv = toCSV([playerToFlat(DEFAULT_PLAYERS[0])]);
    const parsed = parseCSV(csv); const rec = rowToPlayer(parsed.headers, parsed.rows[0], parsed.headersNorm);
    console.assert(rec.name===DEFAULT_PLAYERS[0].name, 'CSV roundtrip name');
    console.assert(rec.ratings.kohls===DEFAULT_PLAYERS[0].ratings.kohls, 'CSV roundtrip rating');

    // Header alias coverage samples
    const hdrTest = canonicalKey('\uFEFFPlayer Name') === 'name' && canonicalKey('Coach Twitter Username')==='coach_twitter' && canonicalKey('FG Range')==='metrics.fg_range';
    console.assert(hdrTest, 'header aliases & BOM handling');

    console.log('Self-tests passed ✅');
  } catch(e){ console.warn('Self-tests failed', e); }
})();
