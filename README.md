# First-hands-with-Triton
Solve **CrackMe.c** example with symbolic execution. Read original way at: http://ak42.io/2016/09/26/solving-a-crack-me-with-triton-and-pin-a-k-a-the-lazy-way/

```java
	@Override
	public ACTION_OP onInstruction(BPPath path, long address, Instruction ins) {
		String strInst = getInst(address, ins);
		strInstLine = String.format("0x00%s\t%s", Long.toHexString(address), strInst);

		//System.out.println(strInstLine);
		//.text:0040153B	mov     dword ptr [esp], offset aS ; "%s"
		//.text:00401542	call    _scanf
		if (address == 0x00401542) {
			long szBuffer = ((LongValue) register.getRegisterValue("eax")).getValue();
			addrInput = szBuffer;
			System.err.println(" - SymbolicVar create: 0x" + Long.toHexString(szBuffer));
			for (int i = 0; i < n; i++) {
				String varName = String.format("k%d", i);
				SymbolValue symVar = new SymbolValue(varName);
				symVar.setType(SymbolValue.Type.CHAR);
				memory.setByteMemoryValue(szBuffer + i, symVar);
			}
			rule.generateNextInstructionForce(ins, path);
		} else if (address == 0x04018fb) {
			// puts
			Formulas pathCond = path.getPathCondition();
			//printFomula(pathCond);
			SolverResult solver = pathCond.getResult();
			System.err.println(" =>> FLAG: \"" + _getSymbolicString(solver) + "\"");
		}
		return ACTION_OP.NOP;
	}
```

A part of results
```
 =>> FLAG: "Tr!`0Nz[ck5"
 =>> FLAG: "T`!v0$~YcA5"
 =>> FLAG: "Tv!z>DlimWC"
 =>> FLAG: "UZ&z$22y|;J"
 =>> FLAG: "Tc!F0?*Ac\5"
 =>> FLAG: "Um()$It(~RJ"
 =>> FLAG: "Tr!t0NRock5"
 =>> FLAG: "UP"\$@n[xIJ"
 =>> FLAG: "Tv!U0RRPco5"
 =>> FLAG: "Tr!t?Naock5"
 =>> FLAG: "Uh"\$T~StaF"
 =>> FLAG: "TH!h'$accA5"
 =>> FLAG: "U{7m$S\dyp6"
 =>> FLAG: "Ur3z$>Ri}S>"
 =>> FLAG: "Un"u$`<t~cT"
 =>> FLAG: "Tp!d(LJ_ci5"
 =>> FLAG: "Um$N$Y~EvfF"
 =>> FLAG: "Ul1x$H^sse6"
 =>> FLAG: "U_&M$'RDl@:"
 =>> FLAG: "U_46$+r%~@>"
 =>> FLAG: "Vs!1#Y'0qhI"
 =>> FLAG: "Vt3P&@'Oco)"
 =>> FLAG: "Ud#s$@ThyIJ"
```
