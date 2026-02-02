<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:39FF14,100:00F3FF&height=220&section=header&text=SYSTEM_OVERRIDE:_ANTONIO&fontSize=60&fontAlign=50&fontAlignY=35&desc=Fullstack%20Dev%20//%20Binus%20University%20//%20PT%20CSA&descAlign=50&descAlignY=65&animation=fadeIn&fontColor=ffffff&stroke=000000&strokeWidth=2" width="100%" alt="Cyberheader"/>
  
  <br/>

  <a href="https://git.io/typing-svg">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=22&pause=1000&color=39FF14&background=00000000&center=true&vCenter=true&width=600&lines=INIT_SEQ..._Loading_Profile_Data;CLASS:_Technomancer+(Fullstack+Dev);GUILD:_BINUS_University+(CS_Dept);CURRENT_OPS:_Building_LMS_%26_Thesis_Research;OS_STATUS:_Ricing_Arch_Linux_(Omarchy)" alt="Typing SVG" />
  </a>
</div>

<br />

<table align="center" border="0" cellpadding="0" cellspacing="0">
  <tr>
    <td width="50%" valign="top">
      <h3 align="center" style="color: #39FF14;">⚡ SYSTEM METRICS</h3>
      <div align="center">
        <img src="https://github-readme-stats.vercel.app/api?username=AntonioSulistio&show_icons=true&theme=tokyonight&hide_border=true&bg_color=0d1117&title_color=39FF14&icon_color=00F3FF" alt="Antonio's Stats" width="100%" />
        <br/><br/>
        <img src="https://github-readme-streak-stats.herokuapp.com/?user=AntonioSulistio&theme=tokyonight&hide_border=true&background=0d1117&ring=39FF14&fire=00F3FF&currStreakNum=ffffff" alt="Streak" width="100%" />
      </div>
    </td>
    <td width="50%" valign="top">
      <h3 align="center" style="color: #00F3FF;">🛠 TECH ARSENAL</h3>
      <div align="center">
        <img src="https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white" />
        <img src="https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white" />
        <br/>
        <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
        <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black" />
        <br/>
        <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white" />
        <img src="https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white" />
        <br/>
        <img src="https://img.shields.io/badge/Arch_Linux-1793d1?style=for-the-badge&logo=arch-linux&logoColor=white" />
        <img src="https://img.shields.io/badge/Waybar-000000?style=for-the-badge&logo=linux&logoColor=white" />
        <br/><br/>
        <a href="mailto:antoniosulistio2004@gmail.com">
          <img src="https://img.shields.io/badge/LINK_UPLINK-GMAIL-c14438?style=flat&logo=gmail&logoColor=white" />
        </a>
        <br/>
        <a href="https://www.linkedin.com/in/antoniosulistio">
          <img src="https://img.shields.io/badge/NET_LINK-LINKEDIN-0077b5?style=flat&logo=linkedin&logoColor=white" />
        </a>
      </div>
    </td>
  </tr>
</table>

<br/>

<div align="center">
  <img src="https://user-images.githubusercontent.com/74038190/212284100-561aa473-3905-4a80-b561-0d28506553ee.gif" width="100%" style="border-radius: 10px; opacity: 0.9;" />
</div>

<h3 align="center">📟 MISSION LOG [ACTIVE]</h3>

```bash
root@antonio-pc:~$ cat ./current_objectives.txt

> [WORK_MAIN_QUEST] ... PT CSA Learning Management System (LMS)
  Target: Optimization & Dashboard Reporting
  Status: [████████░░] 80% // Developing PHP/Laravel Modules

> [ACADEMIC_QUEST] .... Thesis (Skripsi)
  File: "Analisis Efektivitas Penggunaan LMS terhadap
         Peningkatan Kemampuan Kognitif Binus Kemanggisan"
  Status: [███░░░░░░░] 30% // Proposal Drafting & Advisor Sync

> [SIDE_QUEST] ........ Crypto Analytics Dashboard
  Stack:  React + TypeScript + WebSocket
  Status: [████░░░░░░] 40% // Integrating Real-time API

> [SECURITY_OP] ....... Pentest Defense Protocol
  Target: Mitigating XML Attacks & API Vulnerabilities

> [AFK_ACTIVITY] ...... "Where Winds Meet" & Toram Online
<div align="center"> <img src="https://www.google.com/search?q=https://github.com/AntonioSulistio/AntonioSulistio/blob/output/github-contribution-grid-snake.svg" alt="Snake Animation" width="100%"/> </div>


-----

### 📂 File 2: `.github/workflows/snake.yml`

**Crucial Step:** For the snake at the bottom to work, you must create this file.

1.  In your repo, click **Add file** -\> **Create new file**.
2.  Type the name exactly: `.github/workflows/snake.yml`
3.  Paste this code:

<!-- end list -->

```yaml
name: Generate Snake

on:
  schedule:
    # Runs every 6 hours
    - cron: "0 */6 * * *" 
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - uses: Platane/snk/svg-only@v3
        id: snake-gif
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
            
      - uses: crazy-max/gh-action-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
