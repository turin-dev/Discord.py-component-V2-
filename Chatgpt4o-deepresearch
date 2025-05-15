Discord.py 컴포넌트 V2 완벽 가이드 (구조, 사용법 및 예제)

Discord 봇 개발에서 **메시지 컴포넌트(Message Component)**는 버튼, 드롭다운(셀렉트 메뉴), 모달 등과 같이 사용자와 상호작용할 수 있는 UI 요소입니다. Discord.py 라이브러리 v2 버전부터는 이러한 컴포넌트를 공식적으로 지원하며, 2025년에는 컴포넌트 V2로 불리는 새로운 시스템이 도입되어 한층 강력한 UI 구성과 상호작용이 가능해졌습니다. 아래에서는 컴포넌트 V2의 개념과 구조, 지원되는 UI 요소와 사용 방법, 컴포넌트별 예제 코드, 컴포넌트 V1 vs V2 차이점과 마이그레이션, 이벤트 처리 및 사용자 입력 대응 방법, 버전별 변경 사항, 공식 문서 및 참고 자료를 순서대로 정리합니다.

1. 컴포넌트 V2의 구조와 설계 개념

컴포넌트 V2는 Discord가 2025년 도입한 새로운 메시지 컴포넌트 시스템으로, 기존(컴포넌트 V1) 대비 메시지 레이아웃과 상호작용 요소에 대한 제어권이 크게 향상되었습니다. 컴포넌트 V1에서는 메시지에 텍스트 콘텐츠와 임베드(embed)를 포함하면서 추가 인터랙션 요소를 액션 행(Action Row) 단위로 첨부했습니다. 예를 들어 최대 5개의 버튼을 한 행에 넣고 최대 5행까지(총 25개 요소) 추가할 수 있었죠. 반면 컴포넌트 V2에서는 메시지의 본문 전체를 컴포넌트로 구성하기 때문에 일반 content나 embed 필드를 사용하지 않으며, 오직 컴포넌트들로만 메시지를 구축합니다.

컴포넌트 V2의 주요 설계 개념은 다음과 같습니다:

전체 메시지 컴포넌트화: 메시지 내용을 텍스트 표시 컴포넌트로 넣고, 이미지나 미디어는 미디어 갤러리 컴포넌트로 넣는 등, 기존의 임베드 기능을 컴포넌트 형태로 대체합니다. 따라서 V2 메시지에서는 content, embeds를 설정할 수 없으며, 모든 내용이 컴포넌트로 취급됩니다. 대신 텍스트 표시(Text Display) 컴포넌트로 텍스트를 보여주고, 섹션(Section) 컴포넌트로 텍스트+썸네일/버튼 묶음을 만들며, 미디어 갤러리(Media Gallery) 컴포넌트로 여러 이미지나 영상을 첨부하는 식입니다.

새로운 컴포넌트 타입 도입: 컴포넌트 V2에서는 기존 버튼, 셀렉트 메뉴 외에 7가지의 새로운 컴포넌트 타입이 추가되었습니다. 주요 신규 컴포넌트로는 섹션, 텍스트 표시, 썸네일, 미디어 갤러리, 파일, 구분선(Separator), 컨테이너(Container) 등이 있습니다:

섹션(Section): 여러 줄의 텍스트를 포함하고, 오른쪽에 썸네일 이미지나 버튼 등의 액세서리를 배치할 수 있는 블록. 예를 들어 "공지 사항" 텍스트와 우측의 "확인" 버튼을 하나의 섹션으로 묶을 수 있습니다.

텍스트 디스플레이(Text Display): 단순 텍스트를 표시하는 컴포넌트로 최대 4000자까지 넣을 수 있습니다. (섹션 내부의 텍스트도 이 한도에 포함)

썸네일(Thumbnail): 섹션 등에 넣을 수 있는 작은 이미지 컴포넌트입니다.

미디어 갤러리(Media Gallery): 여러 이미지나 영상을 묶어 하나의 갤러리로 보여주는 컴포넌트입니다. 원격 URL이나 첨부 파일(attachment://)을 통해 미디어를 지정할 수 있습니다.

파일(File): 개별 파일을 표시하는 컴포넌트로, 이미지/영상 외 임의 파일도 첨부 가능하나 텍스트 파일의 프리뷰 등은 지원되지 않습니다. 스포일러 처리도 가능합니다.

구분선(Separator): 컴포넌트 사이에 공간을 넣는 구분선으로, 작은 간격 또는 큰 간격(텍스트 한 줄 또는 두 줄 높이)에 해당하는 두 가지 크기가 있습니다. 기본은 투명하지만 divider=true로 설정하면 선으로 표시됩니다.

컨테이너(Container): 다른 컴포넌트를 담는 용도의 컨테이너로, 하나의 작은 임베드처럼 배경 색상을 입히거나 전체를 스포일러 처리(블러)할 수 있습니다. 컨테이너 내부에는 액션 행 없이 최대 10개의 컴포넌트를 담을 수 있으며, 컨테이너 자체를 중첩하여 넣을 수는 없습니다 (다른 컨테이너는 내부에 추가 불가).


향상된 레이아웃 유연성: 컴포넌트 V2에서는 더 이상 모든 상호작용 요소를 액션 행에 넣을 필요가 없습니다. 기존 V1에서는 라이브러리가 자동으로 5개 요소마다 행을 구성하거나 직접 row 인덱스를 지정해야 했으나, V2에서는 이러한 제약이 완화되어 컨테이너나 섹션 등의 내부에 자유롭게 배치합니다. 또한 메시지당 상위 컴포넌트 개수가 5개에서 10개로 증가하고, 중첩된 전체 컴포넌트 개수도 25개에서 30개로 증가했습니다.

하위 호환 및 호환성: 컴포넌트 V2는 선택 사항으로, 기존 방식(V1)도 여전히 사용할 수 있습니다. 다만 한 번 V2로 생성된 메시지는 V1으로 되돌릴 수 없고, V2 플래그가 있는 메시지는 content나 embed 편집이 불가능하며 오직 컴포넌트로만 편집할 수 있습니다. 이는 Discord 측의 의도된 동작으로, V2 메시지를 편집할 때도 컴포넌트 추가/제거나 내용 교체만 가능합니다. (첨부 파일은 가능하지만, 첨부한 파일도 반드시 컴포넌트에서 참조해야 합니다.)


> 요약: 컴포넌트 V2는 메시지 구성을 컴포넌트 중심으로 재설계하여, 기존보다 풍부한 UI 요소(텍스트/이미지/파일 등)와 더 많은 상호작용 컴포넌트를 활용할 수 있게 합니다. 단, V2 메시지는 본문과 임베드를 따로 사용하지 않고 모두 컴포넌트로 구성하며, 한 번 V2로 설정하면 다시 기존 방식으로 편집할 수 없습니다.



2. 지원되는 UI 요소와 구조 (버튼, 셀렉트 메뉴, 모달 등)

이제 Discord.py에서 사용 가능한 주요 UI 컴포넌트들과 그 구조 및 사용법을 살펴보겠습니다. Discord의 상호작용 컴포넌트에는 버튼(Button), 셀렉트 메뉴(Select Menu) 그리고 모달(Modal) 등이 있으며, Discord.py v2에서는 이들을 편리하게 사용할 수 있도록 discord.ui 하위 클래스와 데코레이터를 제공합니다. 각 컴포넌트별 특징과 구조, 사용 방법은 아래와 같습니다.

버튼(Button)

버튼은 사용자가 눌러서 상호작용을 발생시키는 클릭 가능한 UI 요소입니다. Discord.py에서 버튼은 discord.ui.Button 클래스로 표현되며, 다음과 같은 속성을 가집니다:

label: 버튼에 표시할 텍스트 (최대 80자)

style: 버튼의 스타일/색상 (discord.ButtonStyle 열거형으로 제공되며, 기본값은 회색 secondary 스타일입니다). 주요 스타일은 기본(blurple, primary), 초록(success), 회색(secondary), 빨강(danger), 링크(link) 등이 있습니다.

emoji: 텍스트 대신 이모지를 넣을 경우 지정 (텍스트와 함께 넣을 수도 있음)

custom_id: 버튼의 고유 ID 문자열로, 클릭 시 어떤 버튼인지 식별하는 값입니다. 링크 버튼이 아닌 경우 필수이며 100자까지 가능.

url: 만약 이 버튼이 외부 링크를 여는 버튼이라면 custom_id 대신 URL을 지정 (이 경우 봇에게 상호작용 이벤트가 오지 않음)

disabled: True로 설정하면 버튼을 비활성화(gray 아웃)시켜 클릭을 막을 수 있습니다.

row: 뷰(View) 내에서 버튼이 위치할 행 번호 (0~4). 지정하지 않으면 자동 배치되며, 한 행에는 최대 5개의 버튼이 들어갑니다.


구조: Discord.py에서는 개별 버튼을 직접 메시지에 추가하지 않고, 뷰(View) 내에 포함시킵니다. 뷰는 discord.ui.View 클래스이며 하나의 뷰에 여러 버튼이나 셀렉트 등을 담아 한꺼번에 메시지에 첨부합니다. 버튼은 discord.ui.Button으로 직접 생성해 View.add_item()으로 추가하거나, 더 편리하게는 View 클래스 내부에 데코레이터를 사용해 정의할 수 있습니다:

class MyView(discord.ui.View):
    @discord.ui.button(label="눌러보세요", style=discord.ButtonStyle.primary)
    async def click_me(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("버튼이 눌렸습니다!", ephemeral=True)

위 코드에서는 MyView 클래스에 @discord.ui.button 데코레이터를 사용하여 버튼을 정의했습니다. 데코레이터의 인자로 버튼의 레이블과 스타일 등을 지정하면, 라이브러리가 해당 속성으로 Button 객체를 만들어 뷰에 추가해줍니다. 그리고 click_me 콜백 메서드는 버튼이 클릭될 때 자동으로 호출되며, interaction 매개변수를 통해 상호작용 컨텍스트에 접근합니다. 이 예시에서는 버튼 클릭 시 "버튼이 눌렸습니다!"라는 메시지를 **ephemeral(사라지는 메시지)**로 응답하고 있습니다. ephemeral=True로 설정하면 해당 응답은 해당 사용자에게만 보이고 다른 이용자에게는 보이지 않습니다.

> 참고: Discord.py v2.0 이후 콜백 시그니처는 (self, interaction, button) 순서로 정의해야 합니다 (초기 프리뷰 버전에선 반대 순서였으나 수정됨). 위 예시는 올바른 순서로 작성되었습니다.



버튼 사용 예제: 다음은 간단한 버튼을 메시지에 보내고, 누르면 메시지 내용을 수정하는 예제입니다:

from discord import Interaction, ButtonStyle
from discord.ui import View, button

class CounterView(View):
    def __init__(self):
        super().__init__(timeout=180)  # 3분 후 타임아웃
        self.count = 0

    @button(label="카운트: 0", style=ButtonStyle.red)
    async def count_button(self, interaction: Interaction, button: discord.ui.Button):
        # 버튼 클릭 시 호출되는 콜백
        self.count += 1
        button.label = f"카운트: {self.count}"
        # 카운트가 5 이상이면 버튼 색상을 초록색으로 변경
        if self.count >= 5:
            button.style = ButtonStyle.green
        # 메시지의 뷰를 업데이트 (버튼의 레이블/스타일 갱신을 반영)
        await interaction.response.edit_message(view=self)

    async def on_timeout(self):
        # 타임아웃 발생 시 (180초 동안 클릭 없을 경우) 버튼을 비활성화
        for child in self.children:
            if isinstance(child, discord.ui.Button):
                child.disabled = True
        # 타임아웃 시 메시지를 편집하여 버튼을 모두 비활성화된 상태로 표시
        # (사용자가 눌러도 더 이상 반응하지 않음을 명확히 보여줌)
        await self.message.edit(view=self)

# 예를 들어 슬래시 커맨드 내부 등에서 사용
await interaction.response.send_message("버튼 카운터 테스트", view=CounterView())

위 코드에서 CounterView는 빨간색 버튼을 하나 포함하고 있으며, 버튼을 누를 때마다 레이블의 숫자를 증가시키고 ("카운트: N"), 5에 도달하면 버튼 색상을 녹색으로 바꿉니다. interaction.response.edit_message를 사용하여 **원본 메시지를 편집(edit)**함으로써 버튼의 상태 변화를 실시간으로 업데이트합니다.  또한 View.on_timeout 메서드를 오버라이드하여, 타임아웃 시 버튼을 자동으로 비활성화하도록 구현했습니다. 뷰를 생성할 때 timeout=180으로 지정했으므로, 3분간 아무 상호작용이 없으면 on_timeout이 호출되고, 여기서 버튼의 disabled 속성을 True로 설정한 뒤 메시지를 편집해 뷰를 갱신합니다. 이로써 만료된 버튼을 시각적으로 비활성화하여 사용자가 더 이상 누를 수 없음을 명확히 표시했습니다.

> 팁: 버튼을 비활성화하거나 스타일을 변경하면, 반드시 해당 버튼이 포함된 메시지를 edit_message 또는 interaction.message.edit로 편집해야 변경사항이 사용자 클라이언트에 반영됩니다. 단순히 button.disabled = True로 설정하는 것만으로는 이미 전송된 메시지의 버튼이 바로 업데이트되지 않습니다.



셀렉트 메뉴(Select Menu)

셀렉트 메뉴는 드롭다운 형태의 UI 요소로, 여러 옵션 중 하나 이상을 선택하게 할 수 있습니다. Discord.py에서는 일반 드롭다운을 위한 discord.ui.Select 클래스와, 사용자/역할/채널 등 특수 객체 선택을 위한 여러 Select 하위 클래스 (UserSelect, RoleSelect, ChannelSelect, MentionableSelect)를 제공합니다. 기본적으로 Select 메뉴는 discord.SelectOption들의 리스트를 받아 옵션들을 구성하며, 한 번에 하나의 옵션을 선택하거나 다중 선택도 가능하게 설정할 수 있습니다.

구조: Select 메뉴도 버튼과 마찬가지로 뷰(View)에 아이템으로 추가되어야 합니다. View 클래스 내에 @discord.ui.select 데코레이터를 사용해 간편히 정의하거나, 커스텀 Select 클래스를 정의해 View.add_item()으로 추가할 수도 있습니다. 먼저 데코레이터를 사용하는 예를 보겠습니다:

class SelectView(discord.ui.View):
    @discord.ui.select(
        placeholder="옵션을 선택하세요",  # 선택 전 표시할 문구
        min_values=1, max_values=1,      # 선택 가능한 최소/최대 개수
        options=[                       # 선택지 리스트
            discord.SelectOption(label="옵션 1", description="첫 번째 옵션입니다.", emoji="1️⃣"),
            discord.SelectOption(label="옵션 2", description="두 번째 옵션입니다.", emoji="2️⃣"),
            discord.SelectOption(label="옵션 3", description="세 번째 옵션입니다.", emoji="3️⃣")
        ]
    )
    async def select_choice(self, interaction: discord.Interaction, select: discord.ui.Select):
        # 사용자가 선택을 완료하면 호출되는 콜백
        chosen = select.values[0]  # 선택된 값 리스트 (min_values=1, max_values=1이므로 하나만 선택)
        await interaction.response.send_message(f"**{chosen}** 을(를) 선택하셨습니다.")

위 SelectView 클래스는 뷰가 생성될 때 하나의 셀렉트 메뉴를 자동으로 추가합니다. @discord.ui.select 데코레이터의 인자들을 살펴보면:

placeholder: 사용자가 아직 선택을 하지 않았을 때 셀렉트 박스에 회색으로 표시되는 안내 문구입니다. (위 예에서는 "옵션을 선택하세요")

min_values, max_values: 한 번에 최소/최대 몇 개의 항목을 선택할 수 있게 할지 지정합니다. 기본값은 모두 1이며, max_values를 1보다 크게 지정하면 멀티 선택이 가능한 체크박스 형태로 표시됩니다.

options: discord.SelectOption 객체들의 리스트로, 각 옵션의 상세를 지정합니다. SelectOption은 label(옵션 제목), description(부가 설명), emoji(옵션 왼쪽에 표시할 이모지), value(프로그램 상에서 사용할 값) 등을 인자로 받습니다. 명시적으로 value를 지정하지 않으면 보통 label과 동일한 값이 사용됩니다. 또한 default=True로 설정된 옵션이 있다면, 사용자가 메뉴를 열지 않아도 기본 선택된 상태로 보입니다.


콜백 함수 select_choice에서는 select.values를 통해 사용자가 선택한 옵션들의 값을 리스트로 가져올 수 있습니다. 위 예시는 단일 선택(max_values=1)이므로 첫 번째 값을 select.values[0]으로 꺼내 사용했습니다. 그런 다음 interaction.response.send_message로 선택 결과를 사용자에게 피드백하고 있습니다.

사용자/역할/채널 선택 메뉴: Discord는 2022년 이후 특정 대상 선택용 셀렉트 메뉴를 추가했습니다. 예를 들어 사용자 선택(UserSelect) 메뉴를 사용하면 길드의 멤버 목록이 드롭다운으로 나타나고, 그 중 선택하게 할 수 있습니다. Discord.py에서는 discord.ui.UserSelect, RoleSelect, ChannelSelect, MentionableSelect 클래스로 이러한 컴포넌트를 지원하며, 사용 방법은 기본 Select와 거의 유사합니다. 이들 특수 Select는 options를 수동 지정하지 않으며 Discord 클라이언트가 해당 유형의 객체 리스트를 자동으로 불러와 보여줍니다. UserSelect의 경우 guild에서는 멤버 목록, DM에서는 봇과 자신만 선택 가능하게 동작하며, 선택 결과는 interaction.data나 콜백의 select.values에 Member/User 객체로 반환됩니다. 역할 선택이나 채널 선택도 마찬가지 원리입니다.

특수 Select를 사용할 때도 View에 추가해야 하는 점은 동일합니다. 데코레이터 대신 직접 객체를 추가하는 예시는 다음과 같습니다:

from discord.ui import View, UserSelect

class UserSelectView(View):
    def __init__(self):
        super().__init__()
        # 사용자 셀렉트 메뉴를 생성하여 뷰에 추가
        self.user_select = UserSelect(placeholder="유저 선택...", min_values=1, max_values=3)
        self.add_item(self.user_select)

    @discord.ui.select(default_values=[])  # (데코레이터로 콜백만 정의하는 방법도 있으나 가독성을 위해 분리)
    async def on_user_select(self, interaction: discord.Interaction, select: discord.ui.UserSelect):
        users = [str(u) for u in select.values]  # 선택된 User/Member 객체들을 문자열로
        await interaction.response.send_message(f"선택된 사용자: {', '.join(users)}")

위 예시에서 UserSelect( )를 직접 생성하여 add_item으로 뷰에 추가했습니다. max_values=3으로 설정했으므로 한 번에 최대 3명의 사용자를 선택할 수 있습니다. 콜백에서는 select.values에 담긴 Member 객체들을 꺼내와 이름을 출력했습니다.

> Note: 사용자/역할/채널 선택 컴포넌트는 Discord.py v2.1부터 지원되기 시작했습니다. 또한 v2.4에서는 default_values라는 속성이 추가되어, 처음 메뉴를 표시할 때 미리 선택된 항목을 지정할 수 있게 되었습니다.



제한 및 배치: 하나의 **Action Row(액션 행)**에는 셀렉트 메뉴를 최대 1개만 넣을 수 있고, 버튼은 최대 5개까지 넣을 수 있습니다. Discord.py에서 특별히 row를 지정하지 않으면, 뷰는 가능한 한 각 아이템들을 행에 자동 배치합니다. 만약 하나의 뷰에 버튼과 셀렉트를 함께 추가하면 보통 셀렉트가 한 행 차지하고, 버튼들은 다른 행에 모이게 됩니다. 필요한 경우 각 아이템의 row 속성을 수동 지정하여 배치를 미세 조정할 수 있습니다. 예를 들어 row=0인 버튼들과 row=1인 셀렉트를 만들면 버튼들이 첫 번째 줄에, 셀렉트 메뉴는 두 번째 줄에 위치합니다.

모달(Modal)

모달은 사용자가 입력 필드에 텍스트를 직접 작성하여 제출할 수 있는 팝업 대화창입니다. 예를 들어 봇이 "신청 양식" 모달을 띄워 유저에게 몇 가지 텍스트 입력을 받을 수 있습니다. 모달은 다른 컴포넌트들과 달리 메시지에 바로 첨부되지 않고, 어떤 상호작용(예: 버튼 클릭, 슬래시 명령 실행)에 대한 응답으로 별도의 팝업 창으로 나타납니다. Discord.py v2.0부터 discord.ui.Modal 클래스를 통해 모달을 생성하고 처리할 수 있습니다.

구조: 모달은 하나의 클래스(discord.ui.Modal)로 정의하며, 그 안에 하나 이상의 **텍스트 입력 필드(Text Input)**를 포함합니다. 텍스트 입력 필드는 discord.ui.TextInput 클래스로 표현되고, 모달 클래스의 속성(attribute)으로 정의됩니다. 모달 클래스 자체는 discord.ui.View와 유사하게 동작하지만, View와 달리 메시지에 붙는 것이 아니라 사용자 화면에 즉시 팝업됩니다. 모달에는 제목(title)을 지정할 수 있고 최대 5개의 텍스트 입력을 가질 수 있습니다.

모달 정의 예제:

from discord import ui, TextStyle

class QuizModal(ui.Modal, title="퀴즈 질문"):
    # 모달 제목은 클래스 상속 시 인자로 지정하거나 __init__에서 설정 가능
    name = ui.TextInput(label="이름을 입력하세요")  # 한 줄 텍스트 입력
    answer = ui.TextInput(label="정답을 입력하세요", style=TextStyle.paragraph, placeholder="여기에 정답을 작성하십시오...")

    async def on_submit(self, interaction: discord.Interaction):
        # 사용자가 "제출" 버튼을 눌러 모달을 제출하면 호출됨
        response = f"{self.name.value} 님, 정답을 **{self.answer.value}**(으)로 제출하셨습니다!"
        await interaction.response.send_message(response, ephemeral=True)

위 QuizModal 클래스는 두 개의 텍스트 입력 필드를 포함합니다. name 필드는 단순 한 줄 입력(TextInput 기본 스타일)이고, answer 필드는 여러 줄 입력을 위해 style=TextStyle.paragraph로 지정했습니다. placeholder를 통해 필드에 입력 예시나 안내문구를 넣을 수도 있습니다. 각 TextInput에는 .value 속성이 있어서 사용자가 입력한 내용을 가져올 수 있습니다. 모달이 제출되면 on_submit 콜백이 호출되며, 여기서 입력된 내용을 이용해 응답을 보냅니다. 위 예에서는 제출자의 이름과 답변을 받아 확인 메시지를 ephemeral로 보내주고 있습니다.

모달 표시 (호출)하기: 모달은 프롬프트(prompt) 성격이므로, 반드시 사용자 동작에 대한 응답으로만 나타낼 수 있습니다. 즉, 봇이 임의로 모달을 띄울 수 없고, 슬래시 커맨드나 버튼 클릭 등의 인터랙션 핸들러 내에서 await interaction.response.send_modal(modal_instance)를 호출해야 합니다. 예를 들어, 어떤 버튼 콜백에서 await interaction.response.send_modal(QuizModal())처럼 모달을 보내면 해당 사용자의 디스코드 앱에 모달 폼이 팝업됩니다. 사용자가 입력을 완료하고 "Submit"을 누르면, 봇은 그제서야 QuizModal.on_submit을 통해 값을 받을 수 있습니다.

모달을 띄우는 흐름을 정리하면:

1. 어떤 인터랙션 발생 – (예: /quiz 명령어나 "퀴즈 시작" 버튼 클릭)


2. 봇이 모달 전송 – interaction.response.send_modal(CustomModalClass()) 호출


3. 사용자 입력 후 제출 – Discord가 봇에게 Modal Submit 상호작용 이벤트 전달


4. 모달 클래스의 on_submit 실행 – 입력 값 처리 및 답변 처리



Discord.py에서 모달 제출 이벤트를 별도로 처리할 필요 없이, Modal.on_submit 메서드를 구현하면 자동으로 연결됩니다. 추가로 예외 처리를 위해 on_error(self, interaction, error) 메서드를 오버라이드하여 에러를 처리할 수도 있습니다.

제한: 모달은 텍스트 입력(TextInput) 컴포넌트만 가질 수 있으며 (현재 버튼이나 셀렉트 등을 모달 안에 넣을 수는 없음), 한 모달에 최대 5개 필드까지 추가 가능합니다. 모달의 제목은 최대 45자까지 설정할 수 있습니다. 또한 모달은 보내진 인터랙션에 대해서만 유효하며, 특별한 경우를 제외하면 모달 자체를 미리 등록하거나 재사용하지는 않습니다. (각 모달 인스턴스는 한 번 쓰고 버리는 형태) 슬래시 커맨드의 경우 한 번의 명령으로 모달을 연 뒤, 그 모달의 응답을 같은 명령의 흐름으로 처리할 수 있습니다 (15분 이내).


3. 컴포넌트별 예제 코드 (생성부터 콜백 처리까지)

위에서 각 컴포넌트의 사용법과 간단한 코드 조각을 보였지만, 보다 실전적인 예제 코드를 모아서 다시 정리합니다. Python 코드와 함께 설명을 덧붙여, 실제 Discord.py 봇 코드에서 컴포넌트를 어떻게 생성하고, 어떻게 이벤트를 처리하는지 이해를 돕겠습니다.

버튼 예제 – 간단 투표 버튼: 아래 코드는 "좋아요/싫어요" 투표를 위한 두 개의 버튼과 결과 집계를 구현한 예시입니다. 사용자가 누르면 카운트가 증가하고, 결과가 편집되어 표시됩니다.

import discord
from discord.ext import commands
from discord.ui import View, button

bot = commands.Bot(command_prefix="!")

class VoteView(View):
    def __init__(self):
        super().__init__(timeout=None)  # timeout=None로 설정하여 뷰가 만료되지 않도록 (지속 유지)
        self.upvotes = 0
        self.downvotes = 0

    @button(label="👍 0", style=discord.ButtonStyle.secondary, custom_id="vote_up")
    async def upvote(self, interaction: discord.Interaction, button: discord.ui.Button):
        self.upvotes += 1
        button.label = f"👍 {self.upvotes}"
        await interaction.response.edit_message(view=self)

    @button(label="👎 0", style=discord.ButtonStyle.secondary, custom_id="vote_down")
    async def downvote(self, interaction: discord.Interaction, button: discord.ui.Button):
        self.downvotes += 1
        button.label = f"👎 {self.downvotes}"
        await interaction.response.edit_message(view=self)

@bot.command()
async def 투표(ctx):
    """좋아요/싫어요 투표를 시작하는 명령어"""
    view = VoteView()
    await ctx.send("이 메시지를 보고 의견을 눌러주세요!", view=view)

설명: VoteView는 두 개의 버튼(👍, 👎)을 가지고 있고, 각 버튼을 누를 때마다 클래스에 저장된 upvotes 또는 downvotes를 증가시킵니다. 그리고 버튼의 label을 업데이트한 후 edit_message로 메시지를 갱신하여 버튼 옆의 숫자가 늘어나도록 합니다. custom_id를 수동으로 지정한 것은 (선택 사항이지만) 만약 봇을 재시작하여 VoteView를 새로 생성하지 않고도 이전 메시지의 버튼을 이어서 처리하고 싶을 때를 대비한 것입니다. (이 때는 bot.add_view(VoteView(), message_id=...) 방식으로 재등록 필요) 이 예제에서는 timeout=None으로 설정해 뷰가 만료되지 않도록 했습니다. 따라서 봇이 켜져있는 동안 투표가 무기한 열려있지만, 주의할 점은 봇 프로세스가 재시작되면 기존 뷰 정보가 사라지므로 기존 메시지의 버튼은 동작하지 않게 됩니다.

셀렉트 메뉴 예제 – 역할 선택: 이번 예시는 사용자가 자신의 역할을 선택할 수 있는 셀렉트 메뉴를 제공하는 코드입니다. Discord 서버 내에 "레드", "그린", "블루" 세 가지 역할이 있다고 가정하고, 사용자가 드롭다운에서 하나를 선택하면 봇이 해당 역할을 부여해주는 기능입니다.

class RoleMenu(View):
    @discord.ui.select(
        placeholder="원하는 역할을 선택하세요",
        options=[
            discord.SelectOption(label="레드 팀", value="Red", description="레드 역할을 받습니다.", emoji="🔴"),
            discord.SelectOption(label="그린 팀", value="Green", description="그린 역할을 받습니다.", emoji="🟢"),
            discord.SelectOption(label="블루 팀", value="Blue", description="블루 역할을 받습니다.", emoji="🔵")
        ]
    )
    async def select_role(self, interaction: discord.Interaction, select: discord.ui.Select):
        guild = interaction.guild
        member = interaction.user
        role_value = select.values[0]  # "Red", "Green", "Blue" 중 하나
        role = discord.utils.get(guild.roles, name=role_value)
        if role is not None:
            # 기존에 세 역할 중 하나를 갖고 있었다면 제거
            red = discord.utils.get(guild.roles, name="Red")
            green = discord.utils.get(guild.roles, name="Green")
            blue = discord.utils.get(guild.roles, name="Blue")
            await member.remove_roles(red, green, blue, reason="역할 변경")
            # 새로운 역할 부여
            await member.add_roles(role, reason="사용자 선택")
            await interaction.response.send_message(f"{member.mention}님에게 **{role.name}** 역할을 드렸습니다!", ephemeral=True)
        else:
            await interaction.response.send_message("선택한 역할을 찾을 수 없습니다.", ephemeral=True)

설명: RoleMenu 뷰에는 하나의 셀렉트 메뉴가 정의되어 있습니다. 옵션으로 "레드 팀", "그린 팀", "블루 팀" 세 가지가 주어지고, 각 옵션의 value를 실제 역할 이름 ("Red", "Green", "Blue")으로 설정했습니다. 유저가 선택을 완료하면 select_role 콜백이 호출되고, 선택된 값에 해당하는 역할 객체를 discord.utils.get으로 찾아 부여합니다. 이때 한 사용자가 동시에 둘 이상의 팀 역할을 갖지 않도록, 새로운 역할을 주기 전에 미리 세 팀 역할을 모두 제거하는 로직이 포함되어 있습니다. 응답 메시지는 ephemeral로 전송하여 해당 사용자에게만 역할 부여 결과를 알려줍니다.

이 예시에서는 하나만 선택 가능하도록 (max_values=1 기본값) 설정했지만, max_values를 늘리면 사용자가 여러 역할을 동시에 선택하여 부여받는 것도 가능합니다. 다만 한 번에 너무 많은 역할을 토글하는 것은 혼란을 줄 수 있으므로 일반적으로 1로 두는 편이 좋습니다.

모달 예제 – 피드백 제출 폼: 마지막으로 모달을 이용한 예제입니다. 사용자가 /feedback 슬래시 명령을 입력하면 봇이 모달 창을 띄워서 사용자에게 피드백 제목과 내용을 입력받고, 제출되면 개발자에게 그 내용을 전달한다고 가정해보겠습니다.

from discord import app_commands

class FeedbackModal(discord.ui.Modal, title="피드백 보내기"):
    title_input = discord.ui.TextInput(label="피드백 제목", max_length=50)
    content_input = discord.ui.TextInput(label="피드백 내용", style=discord.TextStyle.paragraph, max_length=300)

    async def on_submit(self, interaction: discord.Interaction):
        feedback_title = self.title_input.value
        feedback_content = self.content_input.value
        # 예시로, 피드백을 특정 채널로 포워딩 (개발자용 피드백 채널 ID가 있다고 가정)
        channel = interaction.client.get_channel(123456789012345678)
        if channel:
            await channel.send(f"📨 새로운 피드백:\n**제목:** {feedback_title}\n**내용:** {feedback_content}\n**보낸이:** {interaction.user}")
        await interaction.response.send_message("피드백이 전송되었습니다. 참여해주셔서 감사합니다!", ephemeral=True)

@bot.tree.command(name="feedback", description="피드백 양식을 제출합니다.")
async def feedback_cmd(interaction: discord.Interaction):
    # 피드백 모달 표시
    await interaction.response.send_modal(FeedbackModal())

설명: FeedbackModal 클래스는 두 개의 텍스트 입력 필드를 정의했습니다. title_input은 한 줄 텍스트 (제목, 최대 50자), content_input은 여러 줄 입력 (내용, 최대 300자)로 구성됩니다. /feedback 명령어가 실행되면 feedback_cmd 함수에서 interaction.response.send_modal(FeedbackModal())을 호출하여 사용자에게 모달을 띄웁니다. 사용자가 제목과 내용을 입력하고 제출하면, FeedbackModal.on_submit 메서드가 호출되어 해당 내용을 받아옵니다. 이 예시에선 봇의 특정 채널(get_channel로 ID 검색)에 피드백 내용을 보내 개발자에게 전달하고, 사용자에게는 감사 응답을 보내주는 흐름입니다.

실제 사용 시에는 피드백 채널 ID를 환경 설정으로 관리하거나, DM으로 전송하는 등 원하는 대로 변경할 수 있습니다. 중요한 점은 모달은 반드시 interaction.response.send_modal로 띄워야 하며, 모달 제출 결과를 처리하는 코드는 on_submit 안에 작성한다는 것입니다. 슬래시 명령과 모달의 연계는 discord.py가 내부적으로 처리해주므로, 별도로 이벤트를 등록하지 않아도 됩니다.


以上の例들을 통해 각 컴ポ넌트를 어떻게 생성하고 상호작용を 처리하는지 볼 수 있었습니다. 다음으로는 컴포넌트 V1과 V2の 차이점を 정리하고、기존 코드を 어떻게 マイ그레이션 할 수 있는지 살펴보겠습니다.

4. 컴포넌트 V1과 V2의 주요 차이점 및 마이그레이션 방법

이 장에서는 **기존 컴포넌트 시스템(V1)**과 새로운 컴포넌트 V2의 차이점을 비교하고, 개발자가 **코드나 설계를 V2에 맞게 전환(마이그레이션)**할 때 유의할 점을 정리합니다. Discord.py v2를 사용하기 전후 또는 Discord API 레벨의 변화 측면에서 살펴보겠습니다.

기능 지원 측면: 컴포넌트 V1에서는 버튼과 셀렉트 메뉴 위주로 비교적 제한된 UI 요소만 사용 가능했고, 모달 기능도 2022년에 API가 추가되기 전까지는 없었습니다. Discord.py 1.x 버전에서는 공식 지원이 없어 서드파티 라이브러리(discord-components 등)를 사용하거나 HTTP 직접 호출을 해야 했습니다. **Discord.py v2.0 (컴포넌트 V1 시대)**부터는 이러한 상호작용 컴포넌트를 공식으로 지원하여 버튼, 드롭다운, 모달 등을 기본 라이브러리만으로 구현할 수 있게 되었습니다. 컴포넌트 V2는 2025년에 나온 Discord API의 업그레이드로, 기존 컴포넌트 기능을 확장하고 메시지 구성 자체에 변화를 준 것입니다. 요약하면:

V1 시대: 버튼, 셀렉트 (옵션형), 모달(텍스트 입력) -- 지원 UI 한정적

V2 시대: 섹션, 텍스트, 썸네일, 미디어 갤러리, 파일, 구분선, 컨테이너 추가 -- 지원 UI 다양화


메시지 구성 방식 차이: 앞서 설명한 대로 V1은 텍스트 콘텐츠 + 컴포넌트 부가 형태였습니다. 봇이 메시지를 보낼 때 content="문자열"이나 embed=Embed(...)로 본문을 보내고, view=View로 컴포넌트를 첨부하는 구조였습니다. 반면 V2에서는 메시지 본문도 하나의 컴포넌트로 간주합니다. 따라서 V2 메시지를 생성할 때는 content나 embeds를 채우지 않고, 대신 예를 들어 컨테이너 컴포넌트를 만들고 그 안에 텍스트 표시 컴포넌트를 추가하는 식으로 프로그래밍적으로 메시지 구조를 구성합니다. Discord.py에서도 장차 이러한 V2 컴포넌트를 다룰 수 있는 새로운 클래스나 메서드를 제공할 것으로 예상됩니다. (예: Message.add_view() 대신 Message.add_component(container) 등의 형태) 현재는 다른 라이브러리에서 add_component_v2 같은 API로 구현 예시가 있는데, 핵심은 기존에는 자동으로 생성되던 Action Row가 V2에선 사라지고, 개발자가 직접 컨테이너/섹션 등을 구성한다는 점입니다.

레이아웃 및 제한 차이: V1에서는 5×5 규칙(최대 5행, 행당 5 컴포넌트)을 기억해야 했고, 뷰 하나에 25개까지 아이템을 넣을 수 있었습니다. V2에서는 10개의 상위 컴포넌트, 최대 30개 중첩 컴포넌트로 한도가 늘어났습니다. 그리고 V1의 액션 행은 V2에서 사실상 컨테이너나 섹션 등의 내부 구조로 대체되므로, 더 유연하게 여러 열(column) 배치나 섹션 분할이 가능합니다. 단, V2는 새 메시징 형식인 만큼 V1 스타일로 만들어진 메시지에는 적용할 수 없고, 처음부터 V2로 보낸 메시지만 해당됩니다. (예를 들어, 기존에 await ctx.send("hi", view=V1View)로 보낸 메시지는 나중에 V2 형식으로 편집 불가)

마이그레이션: 만약 개발자가 Discord.py 1.x에서 2.x로 넘어오면서 컴포넌트를 사용하려는 경우, 코드를 전면 수정해야 합니다. 1.x에서는 아예 공식 지원이 없었으므로 discord-components 등의 라이브러리에서 제공하던 message.components 혹은 wait_for("button_click") 같은 방식을 모두 discord.ui 기반으로 변경해야 합니다. 예를 들어 DiscordComponents(bot) 초기화 코드와 Button(style=..., label=...) 식으로 작성한 부분들은 v2에서는 View와 discord.ui.Button 클래스로 재구현해야 합니다. 다행히 Discord.py v2의 인터페이스는 직관적이라서, 버튼 클릭시의 콜백을 예전처럼 수동으로 대기(bot.wait_for)하지 않고도 데코레이터 기반으로 처리할 수 있어 코드 구성이 쉬워집니다.

한편, 기존 Discord.py v2.0~2.4의 V1 방식 코드를 컴포넌트 V2 방식으로 전환하는 것은 현재(2025년 상반기) 시점에서는 큰 변화가 됩니다. Discord.py 라이브러리에 컴포넌트 V2 관련 인터페이스가 도입되면, 아마도 다음과 같은 작업들이 필요할 것입니다:

메시지 보내는 코드에서 content=나 embed=를 사용했던 부분을 제거하거나, 해당 텍스트/이미지 내용을 컴포넌트 구조로 변환합니다. 예를 들어 content="Hello" 대신 Text Display 컴포넌트를 생성해 넣습니다.

여러 개의 컴포넌트를 그룹화할 필요가 있을 때 ActionRow 대신 Container를 사용하도록 수정합니다. Container는 기본적으로 ActionRow의 역할을 포괄하지만, 색상이나 spoiler 같은 추가 기능이 있으므로 필요에 따라 속성을 설정합니다.

버튼이나 셀렉트 메뉴 추가 시에는 View를 통하지 않고 Container/Section에 직접 추가할 가능성이 큽니다. (예: container.add_component(button) 형태). 이때 기존 @discord.ui.button 데코레이터 방식은 유지되되, 그것이 반환하는 Button 객체를 적절한 컴포넌트 트리에 삽입하는 방법으로 변경될 수 있습니다.

새로운 컴포넌트 타입 활용: V2로 마이그레이션하면서, 기존에 임베드로 이미지를 보여주던 부분을 Media Gallery로 교체하거나, 여러 텍스트+버튼 묶음을 Section으로 재구성하는 등 UI/UX 개선을 함께 진행할 수 있습니다.


Discord.py 공식 깃허브에도 컴포넌트 V2 지원에 관한 이슈와 논의가 진행 중이며, 2025년 4월 기준 컴포넌트 V2 시스템을 구현하는 PR이 올라와 있습니다. 이를 통해 조만간 라이브러리 레벨에서 V2를 직접 활용할 수 있게 될 전망입니다. 따라서 마이그레이션 전략은 우선 기존 V1 방식 코드를 그대로 두더라도, V2가 도입되면 기존 코드는 계속 호환되겠지만 (호환 모드 유지) V2의 향상된 기능을 사용하려면 코드 수정을 통해 새로운 객체와 메서드를 호출해야 한다는 것으로 요약할 수 있습니다.

주의점: 컴포넌트 V2로 전환할 때, 가장 주의해야 할 점은 호환되지 않는 변경이 있다는 것입니다. 특히 V2 메시지는 항상 V2로만 수정 가능하므로, 만약 실수로 V2 플래그 메시지를 보내면 이후 일반적인 message.edit(content=...)나 edit(embed=...) 호출이 실패할 수 있습니다. 그러므로 V2를 도입할 때는, 해당 기능을 사용할 범위(예: 아주 인터랙티브한 특정 메시지)에 한정하고, 일반 출력 메시지는 여전히 content/embed를 쓰는 등의 방식으로 점진적으로 적용하는 것이 좋습니다. Discord.py 레벨에서 이를 얼마나 투명하게 처리해줄지는 추후 문서를 통해 확인해야 합니다.


5. 이벤트 처리 방식 및 사용자 입력 대응 방식

이 절에서는 컴포넌트와 관련된 이벤트가 어떻게 처리되는지, 그리고 사용자 입력에 봇이 어떻게 응답(Respond)하는지에 대해 다룹니다. Discord.py v2에서 컴포넌트 상호작용은 기본적으로 인터랙션(Interaction) 개념으로 처리됩니다. 각 컴포넌트의 사용자 액션(버튼 클릭, 셀렉트 선택, 모달 제출)은 하나의 Interaction 이벤트를 발생시키며, 라이브러리가 이를 View나 Modal 클래스의 콜백으로 연결해주는 구조입니다.

Interaction과 Response: Discord의 모든 상호작용(슬래시 명령, 버튼 클릭, 셀렉트 선택, 모달 제출 등)은 discord.Interaction 객체로 표현됩니다. 이 객체에는 상호작용을 발생시킨 유저 정보, 길드/채널 정보, 상호작용 타입 등이 담겨 있습니다. 또한 interaction.response를 통해 해당 상호작용에 **응답(response)**을 보낼 수 있는 다양한 메서드를 제공합니다. 예를 들어:

interaction.response.send_message() – 새로운 메시지로 응답하기 (버튼 클릭 등에 대한 답장 등)

interaction.response.edit_message() – 원본 메시지를 편집하여 응답하기 (주로 버튼/셀렉트의 경우 UI 업데이트에 사용)

interaction.response.defer() – 응답을 보류(디퍼)하여 생각 중 상태로 만들어두기 (응답 지연이 필요한 경우)

interaction.response.send_modal() – 모달 열기 (해당 인터랙션에 대해 모달을 표시) 등.


이벤트 처리 흐름: Discord.py는 컴포넌트별로 편의 이벤트를 제공하지 않고, View/Modal의 콜백으로 처리하도록 설계되어 있습니다. 예를 들어 버튼을 누르면 on_interaction 이벤트가 발생하지만, 개발자가 직접 이를 잡을 필요 없이 앞서 본 대로 @button 데코레이터로 등록한 메서드가 호출됩니다. 내부적으로 Discord.py는 DiscordClient.on_interaction 이벤트를 수신하여, 그 Interaction이 어떤 View의 버튼에 해당하는지 찾아서 해당 View 객체의 콜백을 실행해줍니다. 만약 매칭되는 View가 없다면 (예: 봇 재시작 등으로 View 객체가 메모리에 없는 경우) on_interaction 이벤트를 수동으로 처리하거나, 사전에 bot.add_view()로 View를 등록해 두어야 합니다. 이 부분은 **Persistent View(영구 뷰)**라고 불리며, 봇이 재시작되더라도 특정 custom_id를 가진 컴포넌트에 대해 콜백을 이어받게 하는 기능입니다. View를 생성할 때 각 Item에 custom_id를 지정하고 timeout=None으로 한 뒤, 봇이 다시 시작될 때 bot.add_view(ViewClass(), message_id=<옛 메시지 ID>) 식으로 등록하면 그 메시지의 상호작용을 다시 받을 수 있습니다.

on_timeout: View에는 on_timeout 메서드 (Modal에는 해당 없음)가 있어, 지정한 timeout 기간 동안 사용자 상호작용이 일어나지 않으면 호출됩니다. 이를 활용하여 타임아웃 시 버튼을 비활성화하거나, 사용자에게 안내 메시지를 추가로 보내는 로직을 구현할 수 있습니다. 예를 들어 위 버튼 예제처럼 on_timeout에서 self.disable_all_items() (모든 child 비활성화) 후 message.edit(view=self)로 갱신하는 패턴이 일반적입니다.

interaction_check: View에는 interaction_check 메서드를 오버라이드하여, 모든 상호작용 이벤트에 앞서 필터링을 할 수 있습니다. 여기서 return True/False를 결정하면, False인 경우 해당 뷰의 콜백들이 실행되지 않고 곧바로 interaction.response.send_message("권한 없음", ephemeral=True) 같은 처리를 할 수 있습니다. 주로 권한 체크나 사용자 체크(예: 이 뷰는 특정 사용자만 눌러야 함)를 위해 사용됩니다.

별도 이벤트: Discord.py는 버튼 클릭 전용 이벤트 (on_button_click) 등을 제공하진 않지만, 모달 제출의 경우는 on_modal_submit 이벤트도 없습니다. 대신 Modal의 on_submit이 있으므로 이것으로 충분합니다. 만약 개발자가 모든 상호작용을 하나에서 처리하고 싶다면 bot.tree.on_error나 bot.on_interaction 등을 활용해볼 수는 있지만, 일반적으로 권장되는 방식은 아닙니다. (특정 상호작용을 수동으로 기다리려면 bot.wait_for("interaction", ...)로 필터링 가능은 하나, View/Modal의 설계를 따르는 편이 낫습니다.)


응답 타입과 제약: Discord API 규칙상 모든 Interaction에는 3초 내에 응답해야 합니다. Discord.py는 이를 위해 콜백 내에서 interaction.response 메서드가 반드시 호출되도록 하는 것을 권장합니다. 만약 3초 내에 응답을 보내지 않으면 해당 인터랙션은 만료되고 사용자에게 "응답하지 않음" 오류가 뜹니다. 시간이 오래 걸리는 작업을 해야 할 경우 interaction.response.defer()를 먼저 호출해 두면 최대 15분까지 응답 시간을 늘릴 수 있습니다. (일반 slash 명령의 Defer와 동일) 버튼이나 셀렉트의 경우 defer 업데이트(DeferredMessageUpdate) 형태로 디퍼해두고 나중에 edit_message를 호출할 수도 있습니다.

또한 응답의 공개 여부(ephemeral)는 최초 응답에서만 결정됩니다. 첫 응답을 ephemeral=True로 했다면 추가로 보내는 followup 메시지도 기본 ephemeral이고, 반대로 공개 응답했다면 중간에 ephemeral로 변경할 수 없습니다. 이 부분은 상호작용 공통 사항입니다.

사용자 입력 처리: 사용자 입력은 크게 버튼/셀렉트 선택과 모달 텍스트 입력으로 나뉩니다. 버튼/셀렉트는 위에서 본 것처럼 Interaction 객체의 interaction.data나 콜백 함수의 인자로 받은 button/select 객체로부터 어떤 버튼이 눌렸는지, 어떤 옵션들이 선택됐는지 등을 알 수 있습니다. 예를 들어 버튼의 경우 button.custom_id로 식별하거나, 하나의 View에 여러 버튼이 있다면 아예 콜백 함수를 각각 만들어 분기할 수도 있습니다. 셀렉트 메뉴는 select.values에 선택된 값들이 리스트로 제공되므로 바로 활용하면 됩니다.

모달의 텍스트 입력은 Modal 클래스의 속성 (self.title_input.value) 형태로 접근하며, 모두 문자열로 반환됩니다. 숫자 입력을 받고 싶으면 입력받은 문자열을 int()로 변환하는 식으로 처리합니다. TextInput에는 min_length, max_length, regex 등의 검증 속성도 있으므로 (이 조건을 만족하지 않으면 Discord 클라이언트에서 제출 버튼 비활성화), 기본적인 검증은 클라이언트 측에서 됩니다.

요청 대기: 가끔 컴포넌트 인터랙션에 대해 봇이 다른 사용자 응답을 기다려야 하는 시나리오가 있을 수 있습니다. 이때는 상태를 저장하고 별도의 이벤트를 기다리는 로직을 짤 수도 있지만, 가능하면 모달을 활용하는 것이 편합니다. 예를 들어 "추가 정보 입력" 버튼을 누르면 모달로 추가 정보를 받게 하거나, 또는 셀렉트 메뉴를 단계별로 나누어 한 번 선택 후 봇이 다른 메시지/셀렉트를 보내는 형태로 구현하는 식입니다.

실패 처리: 상호작용 처리 중 에러가 발생하면 Discord.py는 기본적으로 콘솔에 스택트레이스를 남기고 조용히 넘어갑니다. 필요한 경우 View.on_error(self, interaction, error) 혹은 Modal.on_error를 구현하여 에러 발생 시 사용자에게 사과 메시지를 보내거나 로그 채널에 알림을 남길 수 있습니다.

이벤트 예시 요약: 예를 들어, 사용자가 특정 버튼을 누르면:

1. Discord 서버가 봇에게 InteractionCreate 이벤트를 보냄 (내용: 어떤 메시지의 어떤 custom_id 버튼 클릭, 누가 클릭함 등).


2. Discord.py는 이 이벤트를 받아 해당 custom_id가 속한 View를 찾음 (과거에 ctx.send(view=my_view)로 보냈다면 my_view 객체).


3. View 내에 정의된 버튼 콜백을 실행 (async def button_callback(self, interaction, button): ...).


4. 콜백 내에서 interaction.response로 응답하거나 interaction.message.edit로 수정.


5. 응답이 완료되면 라이브러리가 ACK를 Discord에 보내고 사용자는 클라이언트에서 변화 확인.



만약 해당 View를 찾지 못하면, 기본 설정으로 아무 일도 일어나지 않습니다. 이런 경우 로그에 경고가 나오며, 개발자는 bot.add_view를 사용 안 했거나 custom_id가 안 맞는지 등을 확인해야 합니다.


6. 버전별 릴리스 노트 및 변경 사항 요약

Discord.py 라이브러리의 2.x 버전대에서 컴포넌트와 인터랙션 기능은 지속적으로 강화되어 왔습니다. 여기서는 주요 버전별 변화 중 컴포넌트와 관련된 사항을 간략히 정리합니다:

v2.0.0 (2022년 8월 정식 출시) – Discord.py 2.0은 큰 변화의 시작이었습니다. 이 버전에서 슬래시 명령과 컨텍스트 메뉴 지원, 그리고 메시지 컴포넌트 (버튼, 셀렉트 메뉴) 지원이 공식적으로 추가되었습니다. 개발자는 이제 discord.ui.View, discord.ui.Button, discord.ui.Select 등을 사용하여 인터랙티브한 메시지를 만들 수 있게 되었습니다. 또한 **모달(Modal)**도 v2.0에 초창기 형태로 포함되었는데, Discord의 모달 API가 발표된 직후 라이브러리에 추가되었습니다 (2.0 베타/프리뷰 단계에서 모달이 합쳐짐). 따라서 v2.0부터 이미 discord.ui.Modal과 discord.ui.TextInput을 사용할 수 있었습니다. v2.0에서는 이 외에도 인터랙션 관련 클래스 정비(예: Interaction.response, Interaction.user 등)와, Component 관련 이벤트 (View 시스템) 도입 등이 있었습니다.

v2.1.0 (2023년 초) – Discord.py 2.1에서는 새로운 셀렉트 메뉴 타입들이 추가되었습니다. Discord 업데이트로 사용자, 역할, 채널, 멘션 가능한 대상 선택 메뉴가 도입되었고, 라이브러리도 이를 지원하는 discord.ui.UserSelect, RoleSelect, ChannelSelect, MentionableSelect 클래스를 제공하기 시작했습니다. 개발자는 옵션을 수동으로 나열하지 않아도 되는 셀렉트 컴포넌트를 쓸 수 있게 되었고, 콜백에서 선택된 사용자나 역할 객체 리스트를 바로 사용할 수 있었습니다. 또한 2.1에서는 Context Menu (사용자 또는 메시지 우클릭 메뉴) 명령 지원과, 버튼/셀렉트 관련 몇몇 개선이 이루어졌습니다.

v2.2.0 ~ v2.3.0 – 이 사이 버전들에서는 주로 버그 수정과 내부 개선이 많았고, 컴포넌트 측면의 큰 변화는 적었습니다. 다만 Locale(현지화) 지원이 추가되어, 셀렉트 옵션이나 슬래시 명령어 이름/설명을 여러 언어로 제공할 수 있게 되었습니다. 컴포넌트와 직접 관련은 없지만 사용자 경험 측면에서 중요했습니다. 또한 v2.3 쯤에서 Persistent View 사용 시의 안정성이 개선되었습니다 (봇 재가동 후 View 등록 관련).

v2.4.0 (2024년 초) – Discord.py 2.4에서는 몇 가지 컴포넌트 관련 기능이 확장되었습니다. 앞서 언급한 Select 메뉴의 default_values 지원이 이 버전에 추가되어, 드롭다운 메뉴에서 기본 선택 옵션을 지정할 수 있게 되었고, Button의 SKU ID 지원도 들어갔습니다. SKU ID는 상점 기능과 연동된 버튼 (예: Nitro 구독 버튼 등)에 사용되는 식별자로, 일반적인 봇에서는 거의 쓰이지 않지만 Discord 상점 기능을 다루는 봇이라면 버튼에 sku_id를 넣을 수 있게 된 것입니다. 또한 이 버전에서 **자동 완성 상호작용 (Autocomplete)**과 폼(Form) 제출 등 일부 인터랙션 타입 처리 개선이 있었습니다.

v2.5.0 (2024년 말) – Discord.py 2.5는 비교적 작은 기능 추가와 많은 버그 수정이 이루어진 버전입니다. 주요 추가 사항 중 하나로 메시지 전달(Message Forwarding) 기능이 생겼는데, 이는 컴포넌트와 직접 연관되진 않지만, 봇이 메시지를 다른 채널로 그대로 복사 전송할 수 있는 기능을 지원한 것입니다. 컴포넌트와 관련해서 눈여겨볼 변경은 폴(Poll) 컴포넌트 지원 준비입니다. Discord가 2024년에 도입한 설문(Poll) 메시지 타입을 라이브러리가 지원하기 시작했고, InteractionResponse.edit_message 등에 poll= 매개변수가 추가되었습니다. 이 Poll은 컴포넌트 V2 체계의 일부(섹션의 특별 케이스)로 간주될 수 있는 기능입니다. 아직 discord.py에서 완벽히 노출된 기능은 아니지만, 릴리즈 노트에 따르면 점진적으로 대응하고 있습니다. v2.5에서 눈에 띄는 버그 수정으로는 뷰의 children 초기화 문제나 인터랙션 편의 속성 추가 등이 있습니다.

v2.6.0 이상 (예상) – 2025년 현재 Discord.py의 개발 버전에서는 컴포넌트 V2의 본격 지원이 논의되고 있습니다. 이는 Discord API의 변화에 대응하는 것으로, PR(#10166) 등이 머지되면 Discord.py 2.6 또는 2.7 쯤에서 새로운 컴포넌트 타입들(섹션, 컨테이너 등)을 사용할 수 있게 될 것입니다. 개발자 입장에서는 기존 View/Item 구조와 어떻게 조화를 이룰지 문서를 통해 확인해야 할 것입니다. 컴포넌트 V2는 기존 기능을 대체하기보다는 추가로 제공하는 것이므로, 2.6.x 버전대에서는 기존 코드도 그대로 동작하고, 새로운 컴포넌트는 opt-in 형태로 사용할 수 있을 것으로 예상됩니다. 예를 들어 Message.reply()에 components=[...] 리스트를 넘겨 V2 컴포넌트를 직접 구성하거나, 새로운 UI 클래스들이 제공될 가능성이 있습니다. 공식 문서의 What's New나 변경 로그를 참고하여, 해당 버전 업 시에 변경된 메서드 시그니처나 사용법을 따라주면 될 것입니다.


요약하면, Discord.py 2.x 버전들은 Discord의 인터랙션 API 확장에 발맞춰 컴포넌트 관련 기능을 점차 추가해왔으며, 2.0에서 기본 틀이 마련되고 이후 Select 종류 추가(2.1), 편의 개선(2.4), 새로운 메시지 타입 대응(2.5), 그리고 앞으로 V2 컴포넌트 지원이 이어질 것입니다. 각 버전 업 시 **변경 로그(Changelog)**를 읽어보는 습관을 들이면 라이브러리의 진화 방향을 파악하는데 도움이 됩니다.

7. 공식 문서 및 주요 참고 자료

마지막으로, Discord.py 컴포넌트 V2와 관련하여 도움이 되는 공식 문서와 참고 자료 링크를 정리합니다. 새로운 기능일수록 공식 문서와 Discord 개발자 포럼의 정보를 확인하는 것이 중요합니다.

Discord.py 공식 문서: [Discord.py Interactions API 레퍼런스] – 여기에서 discord.ui 하위 클래스들(View, Button, Select, Modal, TextInput 등)의 사용법과 매개변수를 상세히 확인할 수 있습니다. 예제 코드도 포함되어 있으며, “New in version X.Y” 표시를 통해 해당 기능이 어느 버전에 추가되었는지도 알 수 있습니다.

Discord 개발자 포털 문서: Discord Developers: Message Components 가이드 – Discord API 차원의 컴포넌트 개념과 한계(예: 컴포넌트당 글자수 제한, 메시지당 컴포넌트 개수 제한 등)가 명시되어 있습니다. 특히 컴포넌트 V2 관련 내용(섹션, 컨테이너 등)이 업데이트될 수 있으니 개발자 포럼 게시물이나 변경 로그를 확인하면 좋습니다.

Discord.py GitHub 저장소: Rapptz/discord.py GitHub – 이슈나 PR을 통해 최신 변경 사항을 추적할 수 있습니다. 컴포넌트 V2 구현 PR이나 논의 스레드를 참고하면 라이브러리 차원의 지원 계획을 파악할 수 있습니다. 또한 /examples 디렉토리에 다양한 상호작용 예제가 있으므로 참고하면 유용합니다.

DSharpPlus / DPP 등의 다른 라이브러리 문서: 컴포넌트 V2는 현재 C++ 라이브러리 DPP나 .NET 라이브러리 DSharpPlus에서 먼저 지원을 시작했습니다. 이들의 문서를 보면 V2 기능이 실제로 어떻게 사용되는지 가늠할 수 있습니다. 예컨대 DSharpPlus 문서에는 컴포넌트 V2의 제약과 신규 요소들이 잘 정리되어 있습니다. 이러한 정보를 통해 Discord.py에 해당 기능이 생기면 어떤 형태일지 미리 예상해볼 수 있습니다.

튜토리얼 및 블로그: 한글로 작성된 Discord.py 컴포넌트 입문 글들도 다수 있습니다. 예를 들어 지식 곳간 블로그의 ["디스코드 봇 DIY - 버튼, 드롭다운 메뉴" 포스트][59]에는 한글로 View와 버튼/셀렉트 사용법이 설명돼 있고 코드 예제가 제공됩니다. 다만 이 글은 2024년 기준으로 작성되어 컴포넌트 V2 내용은 없으므로, 기본 개념 학습용으로 활용하시고 최신 기능은 공식 문서를 참조하세요.

Stack Overflow 및 GitHub Gist: 실전에서 마주치는 문제에 대한 Q&A나 코드 조각도 유용합니다. 예를 들어 Stack Overflow의 "Add button components to a message (discord.py)" 질문에는 Discord.py 2.0에서 버튼을 사용하는 법과 예제가 나와 있어 도움이 됩니다. 또 GitHub Gist 중에는 Discord.py v2의 셀렉트 메뉴 사용법을 상세히 설명한 것이 있는데, lykn님의 "selects_or_dropdowns.md"에서 드롭다운의 모든 것을 다룬 자료도 있습니다.

Discord 개발자 서버: Discord.py 공식 서버나 Discord API 서버에서 컴포넌트 V2와 관련된 최신 소식이나 질의응답을 얻을 수 있습니다. 특히 Discord.py 서버에서는 라이브러리 개발자들과 사용자들이 정보를 교환하므로, 궁금한 점을 질문하거나 업데이트 소식을 접하기 좋습니다.


以上を 참고하여 Discord.pyでのコンポーネント V2の活用について深く理解できるでしょう. 최신 공식 문書とコミュニティ情報を継続的にフォローし、 새로운機能を試しながらボット에 적용해 보시기 바랍니다. (마지막 일본어 문장은 잘못 들어간 것으로 보이며, "이상을 참고하여 Discord.py의 컴포넌트 V2 활용에 대해 깊이 이해할 수 있을 것입니다. 최신 공식 문서와 커뮤니티 정보를 지속적으로 팔로우하고, 새로운 기능을 시도하며 봇에 적용해 보시기 바랍니다."로 읽으시면 됩니다.)

참고 자료: Discord.py 공식 문서, DSharpPlus 공식 문서, Discord Developers 가이드, 지식 곳간 블로그 등.

