# Discord.py-component-V2-
Discord.py 컴포넌트 V2 완벽 가이드

Discord.py에서 메시지 컴포넌트 V2는 2025년 도입된 시스템으로, 기존 V1 대비 훨씬 강력하고 유연한 UI 구성을 지원합니다. 본 문서는 컴포넌트 V2의 구조, 사용법, 컴포넌트별 예제, 마이그레이션 방법 등을 정리한 완벽 가이드입니다.

1. 개요 및 구조

전체 메시지 컴포넌트화: content, embed를 제거하고 텍스트/이미지도 컴포넌트로 구성

새 컴포넌트 타입: Section, TextDisplay, Thumbnail, MediaGallery, File, Separator, Container 등 추가

레이아웃 유연성 향상: 행(row) 제약 제거, 10개 상위 컴포넌트, 최대 30개 중첩 가능

하위 호환: V1과 병행 사용 가능, 단 V2 메시지는 V1으로 되돌릴 수 없음


2. UI 요소 정리

버튼 (Button)

클래스: discord.ui.Button

속성: label, style, custom_id, emoji, url, disabled 등

사용법:


class MyView(discord.ui.View):
    @discord.ui.button(label="Click", style=discord.ButtonStyle.primary)
    async def click(self, interaction, button):
        await interaction.response.send_message("Clicked!")

셀렉트 메뉴 (Select Menu)

클래스: discord.ui.Select, UserSelect, RoleSelect, etc.

속성: placeholder, options, min/max_values 등

사용법:


class MyView(discord.ui.View):
    @discord.ui.select(
        placeholder="Choose one",
        options=[
            discord.SelectOption(label="Option 1"),
            discord.SelectOption(label="Option 2")
        ]
    )
    async def select_callback(self, interaction, select):
        await interaction.response.send_message(f"You selected: {select.values[0]}")

모달 (Modal)

클래스: discord.ui.Modal

구성: 최대 5개의 TextInput

사용법:


class MyModal(discord.ui.Modal, title="Feedback"):
    name = discord.ui.TextInput(label="Name")
    message = discord.ui.TextInput(label="Message", style=discord.TextStyle.paragraph)

    async def on_submit(self, interaction):
        await interaction.response.send_message(f"Thanks, {self.name.value}!")

3. 예제 코드

버튼 카운터

class CounterView(View):
    def __init__(self):
        super().__init__()
        self.count = 0

    @button(label="0", style=ButtonStyle.red)
    async def count_button(self, interaction, button):
        self.count += 1
        button.label = str(self.count)
        await interaction.response.edit_message(view=self)

역할 셀렉트

class RoleMenu(View):
    @discord.ui.select(
        placeholder="Choose role",
        options=[
            discord.SelectOption(label="Red", value="Red"),
            discord.SelectOption(label="Green", value="Green")
        ]
    )
    async def choose(self, interaction, select):
        role = discord.utils.get(interaction.guild.roles, name=select.values[0])
        await interaction.user.add_roles(role)
        await interaction.response.send_message(f"Role {role.name} added!", ephemeral=True)

모달 호출

class FeedbackModal(Modal, title="Feedback"):
    text = TextInput(label="Your Feedback", style=TextStyle.paragraph)
    async def on_submit(self, interaction):
        await interaction.response.send_message(f"Received: {self.text.value}")

4. V1 vs V2 차이 및 마이그레이션

항목	V1	V2

텍스트	content	TextDisplay
이미지	embed	MediaGallery
행 구성	ActionRow	Section/Container
수정	자유	컴포넌트만 가능


마이그레이션 팁:

content/embed 제거 → TextDisplay 등 컴포넌트로 대체

ActionRow → Section/Container 사용



5. 이벤트 처리

버튼/셀렉트 → View 콜백 자동 연결

모달 → on_submit으로 처리

타임아웃 처리: on_timeout()

사용자 필터링: interaction_check() 오버라이드


6. 버전별 주요 변경 사항

v2.0: View, Button, Select, Modal 초기 도입

v2.1: UserSelect 등 추가

v2.4: Select.default_values 지원

v2.5: Poll 메시지, SKU 버튼 준비

v2.6~예정: 컴포넌트 V2 본격 지원 예정


7. 참고 자료

Discord.py 문서

Discord 개발자 문서

Rapptz/discord.py GitHub

지식 곳간 블로그 (한글 튜토리얼)



---

문의나 개선 요청은 이슈로 남겨주세요!

