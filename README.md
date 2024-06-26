# 🎨NextJS로 Figma clone 사이트 만들기   

## 🔧초기세팅   
```js
npx create-next-app@latest ./
Need to install the following packages:
create-next-app@14.2.2
Ok to proceed? (y) y
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to customize the default import alias (@/*)? ... No / Yes
```

`npm install fabric uuid @liveblocks/client @liveblocks/react`   

## 🧾개념 정리   
- Next.js   
Next.js는 React 기반의 웹 프레임워크로, 서버 사이드 렌더링(SSR), 정적 사이트 생성(SSG),   
API 라우팅 등의 기능을 제공하여 웹 애플리케이션의 개발을 쉽게 만들어줍니다.   

- Liveblocks   
Liveblocks는 실시간 협업 및 실시간 데이터 동기화를 위한 플랫폼 중 하나입니다. Liveblocks를 사용하면   
여러 사용자가 웹 애플리케이션에서 동시에 작업하고 상호 작용할 수 있습니다. 일반적으로 채팅 애플리케이션,  
협업 도구, 실시간 그림판 등과 같은 다양한 형태의 실시간 협업 애플리케이션을 구축하는 데 사용됩니다.   

- Fabric   
Fabric.js는 HTML5 캔버스를 위한 자바스크립트 라이브러리입니다. 이 라이브러리를 사용하면 웹 애플리케이션에서 그래픽 요소를 만들고 조작할 수 있습니다.   

- tailwindCSS   
Tailwind CSS는 일반적인 CSS 프레임워크와는 달리, 클래스 기반의 스타일링을 강조하여 UI를 쉽게 구축하고 유지보수할 수 있도록 도와줍니다.   

## 🔍주요 기능   
[실시간 마우스 커서]   

liveblocks의 `useOthers()`를 사용하여 실시간 마우스 커서 기능을 구현했습니다.   

```js
const LiveCursors = () => {
    const others = useOthers();

    return others.map(({ connectionId, presence }) => {
        if (!presence?.cursor) return null;

        return (
            <Cursor
                key={connectionId}
                color={COLORS[Number(connectionId) % COLORS.length]}
                x={presence.cursor.x}
                y={presence.cursor.y}
                message={presence.message || ''}
            />
        );
    });
};
```   

[캔버스 기능]   

fabricJS를 이용하여 캔버스 기능을 구현했습니다.   

<details>
  <summary>
  View Code
  </summary>

  ```js
    useEffect(() => {
        const canvas = initializeFabric({ canvasRef, fabricRef })

        canvas.on("mouse:down", (options: any) => {
            handleCanvasMouseDown({
                options,
                canvas,
                isDrawing,
                shapeRef,
                selectedShapeRef,
            })
        });

        canvas.on("mouse:move", (options: any) => {
            handleCanvaseMouseMove({
                options,
                canvas,
                isDrawing,
                shapeRef,
                selectedShapeRef,
                syncShapeInStorage
            })
        });

        canvas.on("mouse:up", (options: any) => {
            handleCanvasMouseUp({
                options,
                canvas,
                isDrawing,
                shapeRef,
                selectedShapeRef,
                syncShapeInStorage,
                setActiveElement,
                activeObjectRef
            })
        });

        canvas.on("object:modified", (options: any) => {
            handleCanvasObjectModified({
                options,
                syncShapeInStorage
            })
        });

        canvas.on("selection:created", (options: any) => {
            handleCanvasSelectionCreated({
                options,
                isEditingRef,
                setElementAttributes
            })
        });

        canvas.on("object:scaling", (options: any) => {
            handleCanvasObjectScaling({
                options, setElementAttributes
            })
        });

        canvas.on("path:created", (options: any) => {
            handlePathCreated({
                options, syncShapeInStorage
            })
        });

        window.addEventListener("resize", () => {
            handleResize({ fabricRef })
        });

        window.addEventListener("keydown", (e: any) => {
            handleKeyDown({
                e,
                canvas: fabricRef.current,
                undo,
                redo,
                syncShapeInStorage,
                deleteShapeFromStorage,
            })
        })

        return () => {
            canvas.dispose();
        }
    }, [])
  ```
</details>   
  
</br>

[채팅 기능]   

TypeScript의 기본적인 기능 중 하나인 열거형을 사용하여 채팅 기능을 구현했습니다.   

```js
 const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
        if (e.key === "Enter") {
            setCursorState({
                mode: CursorMode.Chat,
                // @ts-ignore
                previousMessage: cursorState.message,
                message: "",
            });
        } else if (e.key === "Escape") {
            setCursorState({
                mode: CursorMode.Hidden,
            });
        }
    };
```   

`enter`을 누르면 채팅 모드로 바뀌고, `ESC`를 누르면 숨기기로 바뀝니다.   

## 😱트러블슈팅   
<details>
  <summary>
    npx 캐시 문제
  </summary>   



  ```js
  node:internal/modules/esm/resolve:215
    const resolvedOption = FSLegacyMainResolve(packageJsonUrlString, packageConfig.main, baseStringified);
                          ^
  Error: Cannot find package imported from 
  {
    code: 'ERR_MODULE_NOT_FOUND'
  }

  Node.js v20.12.2   
  ```   

  - 문제 원인   
  캐시에서 충돌이 발생

  - 문제 해결   
  `npx clear-npx-cache` - 캐시를 초기화   
</details>

## 📎사이트   
플랫폼 - [liveblocks](https://liveblocks.io/)   
배포 - [vercel](https://vercel.com/)   
참고 사이트 - [Build and Deploy a Figma Clone](https://www.youtube.com/watch?v=oKIThIihv60)   

## 📘스택   
<div>
  <a href="#"><img alt="Next.js" src="https://img.shields.io/badge/Next.js-000000?style=flat&logo=Next.js&logoColor=white"></a>
  <a href="#"><img alt="Tailwind CSS" src="https://img.shields.io/badge/Tailwind CSS-06B6D4?logo=Tailwind CSS&logoColor=white"></a>
  <a href="#"><img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?logo=TypeScript&logoColor=white"></a>
</div>
