## SSR vs CSR
### SSR(Server Side Rendering)
> 서버에서 렌더링하는 방식. 

- 사용자가 웹페이지를 방문했을 때, 브라우저에서 자바스크립트 코드를 다운 받아 해석할 때까지 기다리지 않고 서버에서 보여질 HTML을 미리 준비해 클라이언트(브라우저)에 응답하는 방식.

- php, jsp 등의 MPA 방식의 렌더링 방식


### CSR(Client Side Rendering)
> 클라이언트에서 렌더링하는 방식.

- 사용자의 요청마다 페이지를 구성하여 응답. 

- React, Angular, Vue 등의 SPA(Single Page Application)들이 이에 해당.

- 데이터가 자주 변하거나 미리 만들어두기 어려운 페이지에 적합

#### SSG(Static Site Generation)
- 서버에서 HTML을 렌더링

- 서버에 미리 구성한 페이지를 요청 시에 응답.

- 캐싱해두면 좋은 페이지에 적합

---

### SPA vs MPA

#### SPA(Single Page Application)

- 하나의 HTML 파일을 기반으로 자바스크립트를 이용해 동적으로 화면의 컨텐츠를 바꾸는 방식의 웹 어플리케이션
- 웹 어플리케이션에 필요한 모든 정적 리소스를 처음에 전부 다운로드하여 그 이후 새로운 페이지 요청마다 갱신이 필요한 데이터만 받아서 클라이언트에서 페이지를 갱신
- AJAX의 등장과 함께 원하는 부분만 클라이언트에서 동적으로 렌더링 가능하고 매번 새롭게 렌더링하지 않아도 되는 SPA을 주로 사용하게 됨.

#### MPA(Multiple Page Application)

- 사용자가 페이지를 요청할 때마다, 웹 서버가 요청한 UI와 필요한 데이터를 HTML로 파싱해서 보여주는 방식의 웹 어플리케이션
- 탭을 이동할 때마다 서버로부터 새로운 HTML을 새로 받아와서 페이지 전체를 새롭게 렌더링하는 전통적인 웹페이지 구성 방식