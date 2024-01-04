---
layout: single
title: "아토믹 디자인"
categories: javascript
tag: [python, blog]
toc: true
author_profile: false
sidebar:
  nav: "docs"
toc: true
---

<script>
export default {
    data() {
        return {
            activityColumns: [
                { property: "scdulCtgryCd", label: "일정카테고리" },
                { property: "scdulCd", label: "일정구분" },
                { property: "ntcCyclCd", label: "알림주기" },
                { property: "wrkjobMgtSeq", label: "업무구분" },
                { property: "regUserId", label: "담당자" }
            ],
            scdulStore: {
                scdulList: [  // 여기에 데이터를 추가합니다.
                    { scdulCtgryCd: "A", scdulCd: "B", ntcCyclCd: "Daily", wrkjobMgtSeq: "C", regUserId: "D" },
                    // 더 많은 데이터 추가 가능
                ]
            },
            // 다른 섹션에 대한 컬럼 정보도 추가 가능
        };
    },
    methods: {
        openScdulMod(row, property) {
            // 해당 컬럼에 대한 동작 수행
            // 예: row와 property를 사용하여 특정 데이터 수정
        }
    }
}
</script>


