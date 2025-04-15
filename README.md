- 코드 중간에 한글 주석 빼기, RenderMonkey에서 에러코드가 잡힌다
- Vertex Shader
```c
/*
전역변수 : 월드행렬, 카메라의 위치 등..

정점데이터 : 정점의 위치, UV 좌표 등..
*/

struct VS_INPUT 
{
   float4 mPosition : POSITION;
};

struct VS_OUTPUT 
{
   float4 mPosition : POSITION;
};

float4x4 gWorldMatrix;         
float4x4 gViewMatrix;          
float4x4 gProjectionMatrix;    


VS_OUTPUT vs_main( VS_INPUT Input )
{
   VS_OUTPUT Output;
   
   /*
   1. 월드 공간에 물체를 카메라 공간으로 이동/회전/확대/축소 - View(뷰 공간)
   2. 2D 이미지 위에 투영 - Projection(투영 공간)
   
   물체공간 ----------> 월드공간 --------> 뷰공간 ---------> 투영공간
              ⅹ월드행렬                   ⅹ뷰행렬                ⅹ투영행렬
   */
   Output.mPosition = mul( Input.mPosition, gWorldMatrix );
   Output.mPosition = mul( Output.mPosition, gViewMatrix );
   Output.mPosition = mul( Output.mPosition, gProjectionMatrix );
   
   return Output;
}
```
![image](https://github.com/user-attachments/assets/e47c4e7f-54a2-4120-911d-1acb1229e397)

- Pixel Shader
```c
// 8bit RGB : 255, 255, 255
// Pixel Shader : 1.0f, 1.0f, 1.0f
// float형태로 0-100
// 함수의 반환 값을 백 버퍼의 색상(COLOR)값으로 
float4 ps_main() : COLOR 
{
   // 4번째 알파값은 투명도
   // float4(r, g, b, a)
   return float4( 1.0f, 0.0f, 1.0f, 1.0f );  
}
```
![image](https://github.com/user-attachments/assets/36778e63-54ed-492a-85be-8185b30f6d31)

