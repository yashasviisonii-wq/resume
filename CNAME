from reportlab.lib.pagesizes import A4
from reportlab.pdfgen import canvas
from reportlab.lib import colors
from reportlab.lib.units import mm
from reportlab.platypus import Paragraph
from reportlab.lib.styles import ParagraphStyle
from reportlab.lib.enums import TA_LEFT, TA_CENTER, TA_RIGHT
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont
import math

W, H = A4  # 595 x 842

# ── BRAND PALETTE ──
BLACK       = (0.06, 0.06, 0.06)
DARK_RED    = (0.42, 0.00, 0.00)
MID_RED     = (0.60, 0.10, 0.10)
BRIGHT_RED  = (0.78, 0.08, 0.08)
OFF_WHITE   = (0.97, 0.95, 0.93)
WARM_GRAY   = (0.55, 0.50, 0.48)
LIGHT_CREAM = (0.96, 0.93, 0.90)
PANEL_BG    = (0.10, 0.04, 0.04)
ACCENT_GOLD = (0.85, 0.72, 0.45)

def rgb(t):
    return colors.Color(*t)

def draw_page(c):
    # ══════════════════════════════════════════
    # BACKGROUND — full black
    # ══════════════════════════════════════════
    c.setFillColor(rgb(BLACK))
    c.rect(0, 0, W, H, fill=1, stroke=0)

    # Subtle grain texture via tiny repeating dots
    c.setFillColor(colors.Color(1, 1, 1, alpha=0.012))
    for row in range(0, int(H), 4):
        for col in range(0, int(W), 4):
            c.circle(col, row, 0.6, fill=1, stroke=0)

    # ══════════════════════════════════════════
    # LEFT SIDEBAR — dark red panel
    # ══════════════════════════════════════════
    sidebar_w = 178
    c.setFillColor(rgb(PANEL_BG))
    c.rect(0, 0, sidebar_w, H, fill=1, stroke=0)

    # Red accent line on sidebar right edge
    c.setFillColor(rgb(BRIGHT_RED))
    c.rect(sidebar_w - 2, 0, 2, H, fill=1, stroke=0)

    # ── SIDEBAR: NAME BLOCK ──
    # Big vertical "Y" watermark letter
    c.saveState()
    c.setFillColor(colors.Color(0.42, 0.00, 0.00, alpha=0.18))
    c.setFont("Helvetica-Bold", 210)
    c.drawString(-18, H - 310, "Y")
    c.restoreState()

    # YASHASVIBE brand name — rotated vertical on far left
    c.saveState()
    c.setFillColor(rgb(BRIGHT_RED))
    c.setFont("Helvetica-Bold", 7.5)
    c.translate(11, H // 2)
    c.rotate(90)
    label = "Y A S H A S V I B E  ·  UGC CREATOR  ·  TECH & AI BRANDS"
    c.drawCentredString(0, 0, label)
    c.restoreState()

    # Vertical red bar on far left
    c.setFillColor(rgb(BRIGHT_RED))
    c.rect(0, 0, 3, H, fill=1, stroke=0)

    # ── SIDEBAR: PHOTO PLACEHOLDER circle ──
    cx = sidebar_w // 2 + 2
    cy = H - 112
    r = 52
    # Gold ring
    c.setStrokeColor(rgb(ACCENT_GOLD))
    c.setLineWidth(1.8)
    c.circle(cx, cy, r + 3, fill=0, stroke=1)
    # Red fill circle
    c.setFillColor(rgb(MID_RED))
    c.circle(cx, cy, r, fill=1, stroke=0)
    # Initials
    c.setFillColor(rgb(OFF_WHITE))
    c.setFont("Helvetica-Bold", 26)
    c.drawCentredString(cx, cy - 9, "YS")
    c.setFont("Helvetica", 7)
    c.setFillColor(rgb(ACCENT_GOLD))
    c.drawCentredString(cx, cy - 22, "YASHASVIBE")

    # ── SIDEBAR: CONTACT SECTION ──
    def sidebar_section_title(label, y):
        c.setFillColor(rgb(BRIGHT_RED))
        c.setFont("Helvetica-Bold", 7)
        c.drawString(18, y, label.upper())
        c.setStrokeColor(rgb(BRIGHT_RED))
        c.setLineWidth(0.5)
        c.line(18, y - 3, sidebar_w - 14, y - 3)

    def sidebar_item(icon, text, y, color=OFF_WHITE, size=7.5):
        c.setFillColor(rgb(BRIGHT_RED))
        c.setFont("Helvetica-Bold", 7.5)
        c.drawString(18, y, icon)
        c.setFillColor(rgb(color) if isinstance(color, tuple) else colors.Color(*color))
        c.setFont("Helvetica", size)
        c.drawString(30, y, text)

    # CONTACT
    y = H - 200
    sidebar_section_title("Contact", y)
    y -= 16
    sidebar_item("@", "ugcyashasvi@gmail.com", y)
    y -= 13
    sidebar_item("▶", "instagram: @yashasvibe", y)
    y -= 13
    sidebar_item("▶", "tiktok: @yashasvibe", y)
    y -= 13
    sidebar_item("▶", "twitter: @yashasvibe", y)
    y -= 13
    sidebar_item("◉", "Based in India", y, WARM_GRAY)

    # STATS
    y -= 28
    sidebar_section_title("Numbers", y)
    stats = [
        ("3+", "Brands"),
        ("6", "UGC Videos"),
        ("20K+", "Views"),
        ("98%", "Satisfaction"),
    ]
    y -= 12
    for val, label_s in stats:
        c.setFillColor(rgb(BRIGHT_RED))
        c.setFont("Helvetica-Bold", 16)
        c.drawString(18, y, val)
        c.setFillColor(rgb(WARM_GRAY))
        c.setFont("Helvetica", 6.5)
        c.drawString(18, y - 9, label_s.upper())
        y -= 26

    # SKILLS
    y -= 8
    sidebar_section_title("Software", y)
    skills = [
        ("Premiere Pro", 85),
        ("After Effects", 90),
        ("Photoshop", 95),
        ("CapCut", 95),
        ("Illustrator", 90),
        ("Lovable AI", 92),
        ("Poppy AI", 85),
        ("Notion", 95),
    ]
    y -= 14
    bar_w = sidebar_w - 36
    for skill_name, pct in skills:
        c.setFillColor(rgb(OFF_WHITE))
        c.setFont("Helvetica", 6.8)
        c.drawString(18, y, skill_name)
        pct_str = f"{pct}%"
        c.setFillColor(rgb(WARM_GRAY))
        c.setFont("Helvetica", 6)
        c.drawRightString(sidebar_w - 12, y, pct_str)
        # bar bg
        y -= 8
        c.setFillColor(colors.Color(1, 1, 1, alpha=0.07))
        c.rect(18, y, bar_w, 4, fill=1, stroke=0)
        # bar fill
        c.setFillColor(rgb(BRIGHT_RED))
        c.rect(18, y, bar_w * pct / 100, 4, fill=1, stroke=0)
        y -= 10

    # PLATFORMS
    y -= 8
    sidebar_section_title("Platforms", y)
    platforms = ["Instagram", "TikTok", "Twitter (X)", "YouTube", "LinkedIn"]
    y -= 13
    for p in platforms:
        c.setFillColor(rgb(BRIGHT_RED))
        c.setFont("Helvetica-Bold", 7)
        c.drawString(18, y, "›")
        c.setFillColor(rgb(OFF_WHITE))
        c.setFont("Helvetica", 7.2)
        c.drawString(26, y, p)
        y -= 11

    # NICHE tags at bottom of sidebar
    y = 42
    sidebar_section_title("Niche", y)
    tags = ["Tech & AI", "Lifestyle", "Beauty"]
    tx = 18
    y -= 14
    for tag in tags:
        tw = c.stringWidth(tag, "Helvetica-Bold", 6.5) + 10
        c.setFillColor(rgb(DARK_RED))
        c.roundRect(tx, y - 2, tw, 13, 3, fill=1, stroke=0)
        c.setFillColor(rgb(OFF_WHITE))
        c.setFont("Helvetica-Bold", 6.5)
        c.drawString(tx + 5, y + 2, tag)
        tx += tw + 5
        if tx > sidebar_w - 14:
            tx = 18
            y -= 18

    # ══════════════════════════════════════════
    # MAIN CONTENT AREA
    # ══════════════════════════════════════════
    main_x = sidebar_w + 22
    main_w = W - sidebar_w - 34

    # ── HERO NAME ──
    y = H - 28
    # Small label above name
    c.setFillColor(rgb(BRIGHT_RED))
    c.setFont("Helvetica-Bold", 7)
    c.drawString(main_x, y, "UGC CREATOR  ·  CONTENT STRATEGIST  ·  TECH & AI")

    y -= 48
    # Giant name — two lines
    c.setFillColor(rgb(OFF_WHITE))
    c.setFont("Helvetica-Bold", 46)
    c.drawString(main_x, y, "YASHASVI")
    y -= 44
    c.setFillColor(rgb(BRIGHT_RED))
    c.setFont("Helvetica-Bold", 46)
    c.drawString(main_x, y, "SONI")

    # Gold underline accent
    y -= 8
    c.setFillColor(rgb(ACCENT_GOLD))
    c.rect(main_x, y, 80, 2.5, fill=1, stroke=0)
    c.setFillColor(rgb(BRIGHT_RED))
    c.rect(main_x + 84, y, 24, 2.5, fill=1, stroke=0)

    # ── TAGLINE ──
    y -= 18
    c.setFillColor(rgb(WARM_GRAY))
    c.setFont("Helvetica-Oblique", 8.5)
    c.drawString(main_x, y, '"Building and creating what I wish existed."')

    # ── DIVIDER ──
    y -= 14
    c.setStrokeColor(colors.Color(1, 1, 1, alpha=0.08))
    c.setLineWidth(0.5)
    c.line(main_x, y, W - 14, y)

    # ── ABOUT ──
    def section_header(title, ypos):
        # small red box + label
        c.setFillColor(rgb(BRIGHT_RED))
        c.rect(main_x, ypos - 1, 3, 11, fill=1, stroke=0)
        c.setFillColor(rgb(OFF_WHITE))
        c.setFont("Helvetica-Bold", 9)
        c.drawString(main_x + 8, ypos, title.upper())
        # thin line extending right
        c.setStrokeColor(colors.Color(1, 1, 1, alpha=0.1))
        c.setLineWidth(0.4)
        c.line(main_x + 8 + c.stringWidth(title.upper(), "Helvetica-Bold", 9) + 8, ypos + 4, W - 14, ypos + 4)
        return ypos - 14

    y -= 6
    y = section_header("About Me", y)

    about = ("I'm Yashasvi Soni — UGC creator and content strategist behind YASHASVIBE. "
             "I specialize in tech & AI brand content: building ads, app walkthroughs, and full "
             "campaigns that feel native, aesthetic, and high-converting. "
             "From hook to CTA — I script, film, edit and deliver content that turns attention into trust.")

    # Manual text wrap
    c.setFillColor(rgb(WARM_GRAY))
    c.setFont("Helvetica", 7.8)
    words = about.split()
    line = ""
    line_h = 12
    for word in words:
        test = line + (" " if line else "") + word
        if c.stringWidth(test, "Helvetica", 7.8) < main_w:
            line = test
        else:
            c.drawString(main_x, y, line)
            y -= line_h
            line = word
    if line:
        c.drawString(main_x, y, line)
        y -= line_h

    # ── BRAND COLLABORATIONS ──
    y -= 10
    y = section_header("Brand Collaborations", y)

    def brand_card(brand, role, period, bullets_list, bx, by, bw, bh):
        # Card background
        c.setFillColor(colors.Color(1, 1, 1, alpha=0.04))
        c.roundRect(bx, by - bh + 12, bw, bh, 4, fill=1, stroke=0)
        # Red top accent bar
        c.setFillColor(rgb(BRIGHT_RED))
        c.roundRect(bx, by - bh + 12 + bh - 4, bw, 4, 4, fill=1, stroke=0)
        # Brand name
        c.setFillColor(rgb(OFF_WHITE))
        c.setFont("Helvetica-Bold", 9.5)
        c.drawString(bx + 8, by - 4, brand)
        # Role pill
        pill_w = c.stringWidth(role, "Helvetica-Bold", 6) + 10
        c.setFillColor(rgb(DARK_RED))
        c.roundRect(bx + 8, by - 17, pill_w, 10, 2, fill=1, stroke=0)
        c.setFillColor(rgb(OFF_WHITE))
        c.setFont("Helvetica-Bold", 6)
        c.drawString(bx + 13, by - 13, role)
        # Period
        c.setFillColor(rgb(ACCENT_GOLD))
        c.setFont("Helvetica-Oblique", 6)
        c.drawRightString(bx + bw - 8, by - 4, period)
        # Bullets
        bullet_y = by - 26
        for b in bullets_list:
            c.setFillColor(rgb(BRIGHT_RED))
            c.setFont("Helvetica-Bold", 7)
            c.drawString(bx + 8, bullet_y, "›")
            c.setFillColor(rgb(WARM_GRAY))
            c.setFont("Helvetica", 6.8)
            # wrap bullet text
            words_b = b.split()
            bl = ""
            first = True
            for word in words_b:
                test = bl + (" " if bl else "") + word
                if c.stringWidth(test, "Helvetica", 6.8) < bw - 24:
                    bl = test
                else:
                    c.drawString(bx + 16, bullet_y, bl)
                    bullet_y -= 9
                    bl = word
                    first = False
            if bl:
                c.drawString(bx + 16, bullet_y, bl)
                bullet_y -= 9

    # Three brand cards side by side
    card_w = (main_w - 10) / 3
    card_h = 92

    brands = [
        ("LOVABLE AI", "UGC Creator", "Active ●",
         ["Conversion-focused videos showcasing AI app-building", "Hook-led walkthroughs for Instagram & TikTok", "Per-view brand deal"]),
        ("POPPY AI", "UGC Creator", "Active ●",
         ["10-reel UGC content plan + full scripts", "Hooks, captions & brand-compliant delivery", "Per-view deal, TikTok & Reels"]),
        ("PICSART", "UGC Creator", "Completed ✓",
         ["Creative tools UGC content", "Aesthetic lifestyle × tech approach", "Delivered on time & on-brand"]),
    ]

    for i, (brand, role, period, buls) in enumerate(brands):
        bx = main_x + i * (card_w + 5)
        brand_card(brand, role, period, buls, bx, y, card_w, card_h)

    y -= card_h + 4

    # ── WHAT I CREATE ──
    y -= 8
    y = section_header("What I Create", y)

    services = [
        ("📹", "UGC Tech Ads", "App demos, product walkthroughs, tech-native content"),
        ("🎬", "Full Campaigns", "Concept → script → film → edit → delivery"),
        ("⚡", "Hook-First Reels", "Scroll-stopping short-form for TikTok & Instagram"),
        ("🖥", "UI Showcases", "Clean, aesthetic interface demonstrations"),
        ("🤖", "AI Brand Content", "Specialist in AI tools & digital products"),
        ("📊", "Content Strategy", "Workflow design & campaign planning for tech brands"),
    ]

    # 2-column grid
    col1_x = main_x
    col2_x = main_x + main_w / 2 + 4
    col_w = main_w / 2 - 8

    for i, (icon, title_s, desc) in enumerate(services):
        sx = col1_x if i % 2 == 0 else col2_x
        # small card
        c.setFillColor(colors.Color(1, 1, 1, alpha=0.035))
        c.roundRect(sx, y - 22, col_w, 30, 3, fill=1, stroke=0)
        c.setFillColor(rgb(BRIGHT_RED))
        c.rect(sx, y + 4, 2, 22, fill=1, stroke=0)
        c.setFillColor(rgb(OFF_WHITE))
        c.setFont("Helvetica-Bold", 8)
        c.drawString(sx + 8, y - 1, title_s)
        c.setFillColor(rgb(WARM_GRAY))
        c.setFont("Helvetica", 6.8)
        c.drawString(sx + 8, y - 12, desc[:50])
        if i % 2 == 1:
            y -= 36

    if len(services) % 2 == 1:
        y -= 36

    # ── PORTFOLIO ──
    y -= 8
    y = section_header("Portfolio", y)

    portfolio_items = [
        ("UI DESIGN", "Clean, intuitive, aesthetic interface content"),
        ("UGC TECH", "App walkthroughs, demos, tech-native content"),
        ("DIGITAL PRODUCTS", "Apps, templates & productivity tool content"),
    ]
    for title_p, desc_p in portfolio_items:
        c.setFillColor(rgb(BRIGHT_RED))
        c.setFont("Helvetica-Bold", 7.5)
        c.drawString(main_x, y, "▸ " + title_p)
        c.setFillColor(rgb(WARM_GRAY))
        c.setFont("Helvetica", 7.2)
        c.drawString(main_x + c.stringWidth("▸ " + title_p, "Helvetica-Bold", 7.5) + 6, y, "— " + desc_p)
        y -= 12

    # Portfolio link
    c.setFillColor(rgb(ACCENT_GOLD))
    c.setFont("Helvetica-Oblique", 7)
    c.drawString(main_x, y - 2, "🔗 yashasviisonii-wq.github.io/yashasvibe")

    # ── BOTTOM FOOTER BAR ──
    footer_h = 26
    c.setFillColor(rgb(DARK_RED))
    c.rect(sidebar_w, 0, W - sidebar_w, footer_h, fill=1, stroke=0)
    c.setFillColor(rgb(ACCENT_GOLD))
    c.setFont("Helvetica-Bold", 7)
    c.drawString(main_x, 10, "AVAILABLE FOR COLLABORATIONS")
    c.setFillColor(rgb(OFF_WHITE))
    c.setFont("Helvetica", 7)
    c.drawRightString(W - 14, 10, "ugcyashasvi@gmail.com  ·  @yashasvibe")

    # Red dot indicator
    c.setFillColor(rgb(BRIGHT_RED))
    c.circle(main_x + c.stringWidth("AVAILABLE FOR COLLABORATIONS", "Helvetica-Bold", 7) + 8, 12.5, 3.5, fill=1, stroke=0)


# ── GENERATE ──
output_path = "/mnt/user-data/outputs/Yashasvi_Soni_Creative_Resume.pdf"
c = canvas.Canvas(output_path, pagesize=A4)
draw_page(c)
c.save()
print("Creative resume generated:", output_path)
