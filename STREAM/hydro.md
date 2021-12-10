---
sort: 4
---

# 4. 수문 과정
## 4.1 수문과정의 모의 ##

수문현상을 모의하는 방법에 따라 유역모델은 개념식을 사용하는 모델과 물리식에 기초한 모델로 구분될 수 있다.일반적으로 많은 유역모델에서 개념식을 사용하여 수문현상을 모의한다. 그 대표적인 방법으로 미국 SCS (Soil Conservation Service)에서 개발한 CN (Curve Number) 기법이 널리 이용되고 있다. SCS-CN 방법은 간편하고 쉽게 적용할 수 있는 장점이 있으나 강우유출 산정 시 시간에 따른 강우강도 및 강우의 토양침투 변화가 고려되지 않는 단점이 있다(Mishra, 2003; Ponce and Hawkins, 1996). 따라서 대규모 유역의 물수지 계산에는 적합하지만 강우특성 변화와 이에 따른 오염물질의 토양 내 변환 및 이동을 자세히 모의해야 하는 경우에는 한계를 가질 수 있다. 이에 반하여, 하나하나의 수문현상을 물리식에 기초하여 표현하는 모델은 개개의 수문현상을 해석할 수 있으므로 소규모 유역에서 상세한 비점오염 유출과정 모의에 적합하다고 할 수 있다. 그러나 물리식에 기초할 경우 소요시간의 증가와 많은 매개변수에 대한 이해와 보정 및 검정이 요구되는 단점이 있다. 본 연구에서 제시한 STREAM은 유역 내 수문현상의 상세한 해석을 위하여 물리식에 기초하여 수문과정을 모의할 수 있도록 설계하였다. STREAM은 수문현상이 발생하는 공간 위치에 따라 격자(cell), 소유역(subcatchment), 채널(channel) 등 총 3개 수준으로 구분한다(Fig. 2). 정방형으로 구분된 각 격자에서는 지표면, 토양층, 대수층에서 발생하는 수문과정을 모의한다. 격자수준의 수문과정은 강우, 강우차단, 증발산, 토양침투,지하수 충전, 지표면 유출, 중간류 유출, 지하수 유출 등으로 구성된다. 소유역의 수문과정은 지표수, 중간류, 지하수의 3개 저장소를 이용하여 표현된다. 각 저장소에는 해당 소유역 내 격자에서 매 연산시간마다 산정된 지표수 유출, 중간류유출, 지하수 충전을 저장하며, 각 저장소로부터 소역역의 말단으로 Muskingum-Cunge 방식에 의해 유출이 발생한다.

![Stream_HydroProcesses](../../images/stream_HydroProcesses.JPG)
<center>Fig. 2. Hydrological processes in STREAM.</center>

각 소유역 간의 상하류 관계는 노트-링크의 네트워크 연결로 정의하여 모의한다. 채널은 하천과 관망으로 구분되며,격자 특성에 따라 하천과 관망이 하나만 또는 동시에 존재할 수 있다. 하천과 관망에서의 물의 흐름은 Muskingum-Cunge 방식에 의해 해석한다. 관망의 최대유량은 사용자가 지정한 값을 사용하거나 입력된 관망 정보를 이용하여 내부적으로 산정하며, 최대유량을 초과한 유량은 격자 특성에 따라 지표면에 저류되거나 하천으로 월류하게 된다. 각 수문과정을 표현하기 위해 사용된 방정식은 Table 2와 같다.

Table 2. Selected equations for hydrologic processes in STREAM
![stream_HydroEquations](../../images/stream_HydroEquations.JPG)


## 4.2 격자에서의 수문 프로세스 ##

STREAM은 정방형의 격자로 유역을 구분하며, 각 격자는 다시 지표면과 수직 아래의 토양층 및 대수층으로 구성된다. 지표면은 사용자가 지정한 불투수지표면비율에 따라 불투수지역과 투수지역으로 구분된다. 투수지역에는 식생이 존재할 수 있는데, 사용자가 지정한 면적비율과 식생 종류에 따라 그 특성이 부여된다. 토양층은 사용자가 부여한 토양 깊이, 토양함수량(saturation, field capacity, wilting point, residual water content), 포화수리전도도, 토양입도 구성비,대공극(macropore) 발달정도 등의 수문 특성이 부여된다. 토양층 아래에는 대수층이 존재하며, 대수층 속성으로는 대수층의 포화수리전도도, 포화도 등의 속성이 요구된다. 

강우 시 강우량의 일부는 식생의 수관(canopy)에 의해 차단된다. 주어진 시기에 수관의 강우 저장능력은 식생의 엽면적지수(LAI; Leaf Area Index)와 비례하는 것으로 가정하였으며, 식생 성장단계 및 농작물 추수에 따른 엽면적지수의 계절변동은 사용자가 지정한 함수식에 의해 정의된다. 수관에 의해 차단되지 않고 토양 표면에 도달하는 수관통과우량(through fall)은 강우량에서 수관차단우량을 제외하여 산정한다.

잠재증발량 및 기준 식생(reference crop)의 잠재증발산량을 산정하기 위하여 STREAM은 Penman 방정식을 이용한다(Shuttleworth, 1993). 수관으로부터의 직접 증발은 증산작용 이전에 잠재증발량의 비율로 발생하는 것으로 가정한다. 일반 식생의 증발산량은 기준 식생의 증발산량에 비례하는 것으로 가정하고 식생계수와 토양수분 제한요인(soil water stress factor)을 감안하여 산정한다. 토양수분 제한요인(kψ)은 토양함수량이 field capacity보다 높을 때에는 1.0이며 토양함수량이 field capacity 이하로 낮아짐에 따라 선형으로 감소하여 wilting point에 이르면 0이 된다. 지표수가 존재하지 않는 경우, 토양에서의 증발은 식생이 없는 지역과 식생이 존재하는 지역을 구분하여 추정한다. 식생이 존재하는 지역에서의 토양 증발의 증발산에 대한 비율은 식생의 엽면적지수에 따라 지수적으로 감소하는 것으로 가정하였다. 지표수가 존재하는 경우에는 토양으로 부터의 증발은 없고 수표면에서 증발이 이루어진다. 수표면으로부터의 증발량은 잠재 증발량으로부터 추정한다.

강우에 의한 지표유출은 토양, 식생 등으로 구성된 투수면과 도시지역의 콘크리트, 아스팔트 등으로 구성된 불투수면을 구분하여 모의한다. 투수면에 비가 내리면 토양층으로 강우가 침투하게 된다. STREAM은 투수면에서의 강우 또는 지표수의 토양침투 과정을 우리나라의 집중적 강우현상을 감안하여 물리식에 기초한 Green-Ampt 방정식(Green and Ampt, 1911)의 Mein-Larson 변형식(Mein and Larson, 1973)을 이용하여 산정한다. 강우강도가 최대 침투율보다 작은 경우에는 모든 강우량이 토양에 침투하며, 강우강도가 최대 침투율보다 큰 경우에는 수치해석 기법을 이용하여 누적 침투량을 계산한 후 누적 침투량을 초과하는 강우량은 전량 침투초과 유출량(infiltration-excess runoff)으로 계산된다. 토양층이 완전히 포화되면 침투율은 0이 되고, 이후의 강우량은 모두 포화초과 유출량(saturation-excess runoff)으로 계산된다. 투수면에서의 지표유출은 다수의 릴(rill)을 통하여 배출된다. 릴의 밀도는 지표유출의 특성을 결정하는 매우 중요한 요인이지만 기후, 지형, 식생, 토양 등 여러 가지 요인에 의해 결정되는 것이어서 일반화하기 매우 어렵다. 이를 감안하여 릴의 밀도는 고정된 값이 아닌 사용자 입력 매개변수로 설정하였다. 릴의 구조는 각 격자마다 다수의 릴이 격자 중심을 향해 직각 방향으로 놓여있다고 가정하며, 이 때 릴의 폭은 Gilley et al. (1990)이 제안한 경험식을 이용하여 계산된다. 릴의 폭이 결정되면 Manning 방정식을 이용하여 릴에서의 유속을 산정한다. 불투수지표면에 내린 비는 우선적으로 불투수지표면의 울퉁불퉁한 요철 부분 중 움푹 파인 곳에 저류되며, 이 부분이 채워진 후에도 계속해서 내린 비는 지표면유출을 형성하게 된다. STREAM에서는 불투수지표면이 저류할 수 있는 강우의 양은 사용자가 입력하도록 하였으며, 저류된 물은 증발에 의해 소모된다. STREAM에서 토양함수량이 field capacity를 초과하면 토양층의 상부는 field capacity를 유지하면서 토양층의 하부로부터 포화되기 시작하는 것으로 가정한다. 토양층의 이러한 부분적 포화는 지하수위가 토양층 내에 위치하는지의 여부와 상관없이 자유수면을 형성하게 되는데, 이 자유수면의 높이가 하상보다 높으면 토양층의 대공극을 통하여 Darcian 유동 형태의 중간류가 발생된다. 중간류 발생의 전제조건인 토양층의 대공극은 식생지역에서만 발달하며 그를 통한 유효수리전도도는 포화수리전도도에 대공극 계수를 곱하여 산정한다. 토양 함수량이 field capacity보다 클 경우에는 대수층 충전이 이루어진다. STREAM에서는 토양층으로부터 대수층으로 충전되는 물의 양은 토양 함수량,토양층의 부피, 대수층까지의 평균 이동시간에 의해 산정된다. 대수층까지의 평균 이동시간(TT)은 평균 유동거리를 불포화 수리전도도로 나누어 계산하며, 불포화수리전도도는 Van Genuchten (1980)이 제안한 식에 따른다. 대수층으로부터 지하수 유출은 Neitsch et al. (2009)이 SWAT 모델에서 제안한 방법에 따라 이전 시간의 지하수 유출과 현재 시간 토양으로부터의 지하수 충전량, 지하수 감수계수(Kgwr)를 이용하여 산정한다.

## 4.3 하천에서의 수문 프로세스 ##

STREAM에서 상하류 소유역 사이의 위계관계는 노드-링크의 연결 구조로 표현된다. 각 소유역의 최종 출구에는 노드가 위치하며, 노드는 소유역의 배수체계에 따라 하천노드 또는 관망노드로 구분된다. 링크는 노드와 노드 사이를 연결하는 개체로, 노드 종류에 따라 하천이나 관망을 대표하게 된다. STREAM에서 링크에서의 물의 흐름은 Muskingum-Cunge 방법에 의해 해석한다. 도시의 관망은 합류식 관거와 분류식 관거로 구분할 수 있으며, 분류식 관거는 오수관거와 우수관거로 나누어진다. 합류식 관거에는 우수와 점오염원이 모두 유입되고, 분류식 관거에서는 우수는 우수관으로, 점오염원은 오수관으로만 유입하는 것으로 가정하였다. 많은 경우 도시 관망 말단에는 차집관거가 있어 관망을 통해 유출되는 유량의 일부를 차집하여 하수처리장으로 보내도록 하는데 사용자가 제시하거나 또는 내부적으로 산정된 차집용량을 이용하여 이를 모의할 수 있도록 모델을 개발하였다. 이를 통해 STREAM에서는 초기우수 발생 시 우수 차집으로 인해 감소하는 유량을 반영할 수 있다.