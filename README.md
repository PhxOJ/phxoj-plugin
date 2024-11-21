Plugin for hydrooj.


# Additional notes

## Change download code to be student ID

`/usr/local/share/.config/yarn/global/node_modules/hydrooj/src/model/user.ts`

- `UserModel`
- Add a new method getUnameByUid
- ```typescript
  @ArgMethod
    static async getUnameByUid(domainId: string, _id: number): Promise<string | null> {
        const user = await this.getById(domainId, _id);
        return user ? user.uname : null;
  }

  ```
  

`/usr/local/share/.config/yarn/global/node_modules/hydrooj/src/handler/contest.ts`
- `export class ContestCodeHandler extends Handler `

- change to be 
- ```typescript
  for (const tsdoc of tsdocs) {
      if (all) {
          for (const j of tsdoc.journal || []) {
              const uname = await user.getUnameByUid(domainId, tsdoc.uid);
              let name = `${uname}_U${tsdoc.uid}_P${j.pid}_R${j.rid}`;
              if (typeof j.score === 'number') name += `_S${j.status || 0}@${j.score}`;
              rnames[j.rid] = name;
          }
      } else {
          for (const pid in tsdoc.detail || {}) {
              const uname = await user.getUnameByUid(domainId, tsdoc.uid);
              let name = `${uname}_U${tsdoc.uid}_P${pid}_R${tsdoc.detail[pid].rid}`;
              if (typeof tsdoc.detail[pid].score === 'number') name += `_S${tsdoc.detail[pid].status || 0}@${tsdoc.detail[pid].score}`;
              rnames[tsdoc.detail[pid].rid] = name;
          }
      }
  }

  ```

