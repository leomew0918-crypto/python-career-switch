import streamlit as st
from datetime import datetime

# ===== 頁面設定 =====
st.set_page_config(
    page_title="愛情回憶錄 · 小瑤", 
    page_icon="💝",
    layout="centered"
)

# ===== 你哋嘅真實資料（已幫你填好）=====
boy_name = "小健"
girl_name = "小瑤"
meet_date = "06-01-2026"
meet_place = "OMI"
dating_date = "25-01-2026"
first_date_place = "睇日落"

# ===== 密碼保護 =====
if "authenticated" not in st.session_state:
    st.session_state.authenticated = False

if not st.session_state.authenticated:
    st.title("🔐 愛情回憶錄 · 只係俾你")
    password = st.text_input("輸入密碼先玩得：", type="password")
    if st.button("進入"):
        if password == "小瑤愛小健":
            st.session_state.authenticated = True
            st.rerun()
        else:
            st.error("❌ 密碼錯，唔俾玩！")
    st.stop()  # 未通過認證就停喺度

# ===== 通過密碼，顯示遊戲 =====
st.success("✅ 歡迎你，小瑤～ 💕")

# ===== 初始化遊戲狀態 =====
if "step" not in st.session_state:
    st.session_state.step = 1
    st.session_state.love_count = 0
    st.session_state.quiz1_date_done = False
    st.session_state.quiz1_place_done = False
    st.session_state.quiz2_date_done = False
    st.session_state.quiz2_place_done = False

# ===== 裝飾標題 =====
st.markdown("✨" * 20)
st.markdown(f"### 💝   {boy_name} 💕 {girl_name}    💝")
st.markdown("### 💝     愛情回憶錄 · 終極考驗     💝")
st.markdown("✨" * 20 + "\n")

# ===== PART 1：愛嘅考驗 =====
if st.session_state.step == 1:
    st.header("💘 PART 1：愛嘅考驗")
    st.warning("⚠️  規則：連續4次都要答「愛」，唔可以斷！")
    st.write(f"{girl_name}，準備好未？\n")

    st.markdown("---")
    
    # 顯示目前進度
    progress = st.session_state.love_count
    st.write(f"💞 目前答咗 **{progress}** 次「愛」")
    
    # 選擇答案
    answer = st.radio(
        "你愛唔愛我？",
        ["", "愛", "唔愛"],
        index=0,
        key="love_radio"
    )
    
    if st.button("💬 提交答案", key="love_submit"):
        if answer == "愛":
            st.session_state.love_count += 1
            if st.session_state.love_count == 1:
                st.success("💖 好乖～繼續！")
            elif st.session_state.love_count == 2:
                st.success("💗 真係？我再問多次！")
            elif st.session_state.love_count == 3:
                st.success("💓 仲有一次咋！")
            elif st.session_state.love_count == 4:
                st.balloons()
                st.success("💕 最後一次！")
                st.success("🌟" * 20)
                st.success("🌟        恭 喜 通 過 考 驗        🌟")
                st.success("🌟" * 20)
                st.markdown(f"**💯 {girl_name}，你連續4次都話愛我！**")
                st.markdown("**💋 我好感動...我都係，永遠愛你！**")
                st.session_state.step = 2
                st.rerun()
        else:
            st.error("😭 嗚...你竟然答唔愛...")
            st.error("❌ 考驗失敗！由頭嚟過！")
            st.session_state.love_count = 0
            st.rerun()

# ===== PART 2：第一次見面 =====
if st.session_state.step == 2:
    st.header("📅 PART 2：我哋嘅第一次見面")
    st.write(f"{girl_name}，考考你仲記唔記得...")
    st.markdown("---")
    
    col1, col2 = st.columns(2)
    
    with col1:
        st.subheader("📌 認識日期")
        if not st.session_state.quiz1_date_done:
            date_answer = st.text_input("我哋第一日認識係幾時？(DD-MM-YYYY)", key="date1")
            if st.button("✅ 提交日期", key="date_submit"):
                if date_answer == meet_date:
                    st.success("✨ 啱啦！你記性好好呀！")
                    st.session_state.quiz1_date_done = True
                    st.rerun()
                else:
                    st.error("❌ 唔啱...")
                    st.info("💡 提示：係冬天嘅日子，一月頭")
        else:
            st.success(f"✅ 你已經答啱：{meet_date}")
    
    with col2:
        st.subheader("📌 認識地點")
        if not st.session_state.quiz1_place_done:
            place_answer = st.text_input("我哋喺邊度認識？", key="place1")
            if st.button("✅ 提交地點", key="place_submit"):
                if place_answer == meet_place:
                    st.success("☕ 冇錯！就係呢度！")
                    st.session_state.quiz1_place_done = True
                    st.rerun()
                else:
                    st.error("❌ 唔係喎...")
                    st.info("💡 提示：可以係手機上面識到朋友㗎")
        else:
            st.success(f"✅ 你已經答啱：{meet_place}")
    
    # 兩個都答啱先可以去下一關
    if st.session_state.quiz1_date_done and st.session_state.quiz1_place_done:
        st.markdown("---")
        st.success(f"✅ 你好叻！我哋係{meet_date}喺{meet_place}認識㗎！")
        st.info("💝 嗰日嘅事，我到而家都記得清清楚楚...")
        if st.button("➡️ 下一關", key="next2"):
            st.session_state.step = 3
            st.rerun()

# ===== PART 3：重要日子 =====
if st.session_state.step == 3:
    st.header("💑 PART 3：我哋嘅重要日子")
    st.markdown("---")
    
    col1, col2 = st.columns(2)
    
    with col1:
        st.subheader("📌 問題1：拍拖紀念日")
        if not st.session_state.quiz2_date_done:
            dating_answer = st.text_input("我哋幾時正式拍拖？(DD-MM-YYYY)", key="dating")
            if st.button("✅ 提交拍拖日", key="dating_submit"):
                if dating_answer == dating_date:
                    st.success("🎉 中呀！你最叻！")
                    st.session_state.quiz2_date_done = True
                    st.rerun()
                else:
                    st.error("❌ 唔啱...")
                    st.info("💡 貼士：係1月尾，2X號左右")
        else:
            st.success(f"✅ 你已經答啱：{dating_date}")
    
    with col2:
        st.subheader("📌 問題2：第一次約會")
        if not st.session_state.quiz2_place_done:
            place_answer = st.text_input("我哋第一次約會去咗邊度？", key="firstplace")
            if st.button("✅ 提交約會地點", key="place2_submit"):
                if place_answer == first_date_place:
                    st.success("🌹 冇錯！我仲記得嗰日你好靚")
                    st.session_state.quiz2_place_done = True
                    st.rerun()
                else:
                    st.error("❌ 唔係喎...")
                    st.info("💡 提示：可以睇到海，好浪漫㗎")
        else:
            st.success(f"✅ 你已經答啱：{first_date_place}")
    
    # 兩個都答啱先去下一關
    if st.session_state.quiz2_date_done and st.session_state.quiz2_place_done:
        st.markdown("---")
        st.success(f"✅ 全部答啱！{dating_date}我哋拍拖，第一次約會去{first_date_place}，")
        st.info("💕 每一個細節我都冇忘記過...")
        if st.button("➡️ 最後一關", key="next3"):
            st.session_state.step = 4
            st.rerun()

# ===== PART 4：情信 =====
if st.session_state.step == 4:
    st.header("💌 PART 4：寫俾你嘅情信")
    st.markdown("---")
    
    st.markdown(f"### 親愛嘅 {girl_name}：\n")
    st.write("多謝你陪我玩完呢個遊戲 💝")
    st.write("我想同你一齊回顧我哋嘅故事：\n")
    
    st.markdown(f"""
    📅 **{meet_date}** - 喺 **{meet_place}** 認識你  
    &nbsp;&nbsp;&nbsp;&nbsp;🩷 嗰日嘅你，笑得好好睇  
    
    💑 **{dating_date}** - 你應承做我女朋友  
    &nbsp;&nbsp;&nbsp;&nbsp;🩷 嗰日嘅我，開心到成晚瞓唔著  
    
    🌊 **{first_date_place}** - 我哋第一次約會  
    &nbsp;&nbsp;&nbsp;&nbsp;🩷 嗰日嘅海風，到而家都記得  
    """)
    
    st.markdown("~" * 40)
    st.markdown("""
    💌 **我想話俾你聽：**  
       💖 由認識你嘅第一日開始  
       💖 你已經係我生活中最重要嘅部分  
       💖 多謝你願意愛我  
       💖 多謝你記得我哋嘅每一個日子  
       💖 我會用一生去對你好  
       💖 永遠永遠  
    """)
    
    st.markdown("✨" * 20)
    st.markdown(f"### 💋 我愛你，{girl_name}")
    st.markdown("✨" * 20)
    
    st.balloons()
    st.snow()  # 浪漫飄雪效果
    
    st.markdown("---")
    st.markdown("🎮 **遊戲完結！多謝你陪我玩～**")
    st.markdown("💝 呢個程式係我親手寫俾你㗎")
    st.markdown("💝 由 OMI 到而家")
    st.markdown("💝 每一步都係為咗令你開心")
    st.markdown("\n" + "=" * 40)
    st.markdown(f"           **{boy_name} 寫給 {girl_name}**")
    st.markdown("=" * 40)
    
    # 再玩一次按鈕
    if st.button("❤️ 玩多次"):
        for key in list(st.session_state.keys()):
            if key != "authenticated":  # 保留認證
                del st.session_state[key]
        st.rerun()