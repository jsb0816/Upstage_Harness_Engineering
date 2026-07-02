# REFERENCE: 보안 체크리스트 (OWASP Top-10 요약 · 정적)

red-team이 하나씩 대입한다.

1. **Broken Access Control** — 인가 우회, RLS 누락(치명 사고 다발), IDOR
2. **Cryptographic Failures** — 평문 저장/전송, 약한 알고리즘, PII 미암호화
3. **Injection** — SQL/NoSQL/명령/LLM 프롬프트 주입
4. **Insecure Design** — 위협 모델링 부재(→ STRIDE로 방어)
5. **Security Misconfiguration** — 기본 크리덴셜, 과도 권한, 디버그 노출
6. **Vulnerable Components** — 오래된 의존성/CVE
7. **Identification & Auth Failures** — 약한 세션·토큰·MFA 부재
8. **Software & Data Integrity** — 검증 없는 역직렬화·CI 공급망
9. **Logging & Monitoring Failures** — 감사로그 부재로 탐지 불가
10. **SSRF** — 서버가 임의 URL 요청

## CVE 스냅샷 관행
- 주요 프레임워크의 알려진 취약 버전 대역은 기각(정적 목록)
- 실시간 CVE 조회는 로드맵(툴 호출 확인 시)
